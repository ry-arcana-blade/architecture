# server1 Rebuild Manifest

**Status:** ACTIVE PLANNING / LIVE HOST CAPTURE COMPLETE THROUGH PROTECTED INVENTORY  
**Canonical repository:** `ry-arcana-blade/architecture`  
**Source host:** 🟩 `ubuntu-cpu-llm`  
**Target rebuilt host:** 🟥 `server1`  
**Evidence date:** 2026-09-26

## Purpose

This document is the durable source of truth for rebuilding the current `ubuntu-cpu-llm` workload as `server1` after the planned storage and OS upgrade.

The goal is a reproducible bare-metal recovery path, not a block-level clone of the current host.

The intended recovery model is:

1. install Ubuntu on the SATA RAID1;
2. bootstrap the host;
3. apply repository-owned configuration/deployment automation;
4. attach and validate the single NVMe work tier and four-NVMe RAID10 durable tier;
5. restore protected credentials and persistent state from a separate secure recovery package;
6. restore PostgreSQL;
7. recreate CI/test/application runtimes from reviewed Git revisions;
8. run full server acceptance validation.

Secret values, private keys, tokens, passwords, and application secret contents must never be committed to this repository.

---

## Current host baseline

Live inventory of 🟩 `ubuntu-cpu-llm` confirmed:

- Ubuntu 26.04.1 LTS
- ASUS PRIME Z390-A
- Intel Core i5-9600K
- approximately 30 GiB RAM
- current root disk: Samsung 970 EVO 1 TB NVMe
- Docker Engine / containerd / Compose installed
- PostgreSQL 18.6, cluster `18/main`
- Gateway self-hosted CI stack active
- Arcana self-hosted validation stack active
- Crochet self-hosted validation stack active
- Arcana and Crochet release-preflight services present
- Arcana host-metrics service present
- Arcana staging/test services present

The protected inventory completed successfully with hostname and sudo guards passing.

---

## Target storage roles

### SATA RAID1 — OS and reconstructable host configuration

Use for:

- Ubuntu OS
- EFI/boot
- packages
- `/etc`
- systemd
- host users/groups
- SSH daemon configuration
- firewall/network configuration
- repository-owned service definitions
- ordinary system logs
- local source/install trees that can be recreated from Git

The OS RAID1 is the recoverable system tier. Application durability must not depend on unique data living only here.

### Single NVMe — high-churn reconstructable work tier

Planned mount root: `/srv/fast`.

Intended contents include:

- Docker/containerd data root
- CI workspaces
- CI checkout material
- compiler/build cache
- Docker build cache
- temporary validation databases
- transient containers
- test scratch
- model/download caches that are replaceable

Current protected inventory sizes include approximately:

- `/var/lib/docker`: 722 MiB
- `/var/lib/containerd`: 15 GiB

These are classified as RECREATE rather than migrate unless a later review identifies a specific durable volume.

### Four-NVMe RAID10 — durable service-data tier

Planned mount root: `/srv/durable`.

Intended contents include:

- PostgreSQL cluster data
- Secure Agent Gateway / self-hosted CI coordinator state
- durable application state
- service-state databases
- backup/restore staging where appropriate

Current durable-state inventory includes:

- `/var/lib/postgresql`: approximately 173 MiB
- `/var/lib/secure-agent-gateway`: approximately 1.1 MiB
- `/var/lib/arcana-staging-operator`: approximately 2 MiB

The CI durable state currently includes:

- `/var/lib/secure-agent-gateway/gateway-ci-coordinator/coordinator.sqlite3`
- `/var/lib/secure-agent-gateway/arcana-validation-coordinator/coordinator.sqlite3`
- `/var/lib/secure-agent-gateway/crochet-validation-coordinator/coordinator.sqlite3`
- `/var/lib/secure-agent-gateway/gateway-ci-telemetry/snapshot.json`

These are RESTORE/MIGRATE candidates, not disposable CI workspace material.

---

## RECREATE from reviewed sources

The following should be recreated rather than copied as opaque host artifacts:

- Ubuntu 26.04 base installation
- required apt packages
- Docker Engine, containerd, Buildx, Compose
- PostgreSQL 18 software/service installation
- custom service users/groups
- Secure Agent Gateway runtime/venv/releases
- Gateway self-hosted CI controller/coordinator/helper/registration/telemetry units
- Arcana self-hosted validation controller/coordinator/helper/registration units
- Crochet self-hosted validation controller/coordinator/helper/registration units
- Arcana Web checkout
- Crochet Design Lab checkout/runtime
- Docker images
- Docker build cache
- CI workspaces
- disposable test databases
- downloadable model/cache material

Where a unit, deployment script, release selector, or runtime exists in a reviewed repository, the repository copy is authoritative. Do not use the live `/etc/systemd/system` copy as the long-term source of truth.

The current reviewed Gateway/Arcana/Crochet CI services execute Python modules from the Secure Agent Gateway virtual environment under `/opt/secure-agent-gateway/venv`; server1 deployment should reconstruct those from reviewed repository tooling.

---

## RESTORE / MIGRATE

### PostgreSQL

Current cluster:

- PostgreSQL 18.6
- cluster: `18/main`
- current data directory: `/var/lib/postgresql/18/main`
- current port: 5432
- current `listen_addresses=*`
- current `wal_level=replica`
- current `max_connections=100`

Current application databases observed:

- `arcana_blade`
- `arcana_blade_test`
- `crochet_design_lab`
- `crochet_design_lab_test`
- `crochet_release_gate`
- `crochet_release_gate_test`
- `postgres`

Current application roles observed include:

- `arcana_blade`
- `arcana_blade_test_runner`
- `crochet_design_lab`
- `crochet_release_gate`

The server1 rebuild must preserve the logical database contents, ownership, grants, role attributes, passwords through a secure restore path, and required extensions/schema state.

Do not copy PostgreSQL data files into the new layout while the source server is running. The final migration procedure must define a consistent backup/restore or controlled shutdown/copy method.

### PostgreSQL access policy

Current `pg_hba` includes:

- local peer authentication
- localhost SCRAM
- Arcana application access from Docker/private RFC1918 ranges
- Crochet application/test access from the Crochet Docker network
- Arcana test-runner access from the Arcana validation network
- Crochet release-gate access from its dedicated validation network

The exact server1 access rules must be reviewed against the final Docker network design and cross-server PostgreSQL requirement before migration. Do not blindly preserve broad network ranges if narrower rules are practical.

### Secure Agent Gateway / CI coordinator state

RESTORE/MIGRATE:

- Gateway coordinator SQLite state
- Arcana validation coordinator SQLite state
- Crochet validation coordinator SQLite state
- telemetry state where continuity is useful
- any release-preflight state proven durable

This state should move to the RAID10 durable tier.

### Credentials and SSH identities

The live host contains protected credentials for:

- Gateway GitHub CI controller
- Arcana validation GitHub controller
- Crochet validation GitHub controller
- Gateway source fetch
- Arcana validation source fetch
- Crochet validation source fetch
- Arcana release preflight
- Crochet release preflight
- Arcana Web deploy identity
- Crochet Design Lab deploy identity
- restricted Gateway/Arcana/Crochet SSH operator/trigger identities

The manifest records only required paths, owners, and modes. Secret contents must be transferred through a separate secure recovery package and must never be placed in Git.

Relevant protected roots include:

- `/etc/secure-agent-gateway/gateway-ci-controller`
- `/etc/secure-agent-gateway/arcana-validation-controller`
- `/etc/secure-agent-gateway/crochet-validation-controller`
- `/etc/secure-agent-gateway/gateway-source-fetch`
- `/etc/secure-agent-gateway/arcana-validation-source-fetch`
- `/etc/secure-agent-gateway/crochet-validation-source-fetch`
- `/etc/secure-agent-gateway/arcana-release-preflight`
- `/etc/secure-agent-gateway/release-preflight`

Most private keys/tokens are currently mode 0600; release-preflight material uses deliberately scoped group-readable modes where required. The rebuild must recreate the intended ownership/mode contract rather than applying a blanket permission scheme.

### Application secrets

RESTORE securely, without Git storage:

- Arcana Django secret
- Arcana PostgreSQL credentials
- Arcana test PostgreSQL credentials
- Crochet Django secret
- Crochet PostgreSQL credentials
- required `.env` material

Application services currently bind these files into containers rather than embedding them in images.

---

## Host policy to recreate deliberately

### Networking

Current live host:

- interface `eno1`
- IPv4 currently acquired through DHCP
- IPv6 DHCP/RA enabled
- interface pinned by MAC address in installer netplan
- NetworkManager is the renderer

The final server1 address/DNS policy remains a design decision. Because server1 will host PostgreSQL for multiple servers, client configuration should prefer a stable internal service/DNS name rather than hard-coding the server hostname everywhere.

### Firewall

Current UFW state: inactive.

Current nftables rules are substantially Docker-generated NAT/filter state and should not be copied as static firewall configuration.

The server1 design must explicitly define host firewall policy before PostgreSQL becomes a shared network service.

### Restricted sudo policy

Current host has bounded NOPASSWD rules for reviewed helper commands used by:

- Arcana Gateway evaluation
- Arcana Gateway staging operator
- Arcana Gateway test trigger
- Crochet Gateway operator

These policies should be recreated from reviewed deployment sources or explicit Ansible role definitions, then validated command-for-command.

---

## Docker topology observed

Current containers include:

### Arcana Blade

- Caddy container using persistent Caddy volumes plus repository Caddyfile bind mounts
- authoring worker with Arcana Django/PostgreSQL secrets bind-mounted
- Arcana web container with Django/PostgreSQL/test-PostgreSQL secrets bind-mounted

### Crochet Design Lab test

- test web container with Django/PostgreSQL secrets bind-mounted

The rebuild must classify named Docker volumes individually before wiping the old host. In particular, Caddy data/config volumes require review to determine whether they contain meaningful persistent state or may be regenerated.

Docker engine state and build cache as a whole are not migration targets.

---

## INVESTIGATE / DESIGN BEFORE CUTOVER

The following remain open:

1. exact SATA RAID1 partition/EFI layout;
2. exact single-NVMe filesystem and mount options;
3. exact RAID10 filesystem and mount options;
4. final `/srv/fast` and `/srv/durable` directory hierarchy;
5. server1 fixed IP / DHCP reservation / internal DNS design;
6. host firewall policy for PostgreSQL and internal control surfaces;
7. final PostgreSQL client allowlist across server0, server1-local containers, Arcana/Crochet services, and future hosts;
8. PostgreSQL backup/restore method and independent backup destination;
9. final secure secret-backup format/location;
10. whether Caddy named volumes need migration;
11. whether current telemetry snapshot/history should be preserved;
12. which old Arcana/Crochet worktrees or release candidates can be retired;
13. whether old pre-Git Arcana backup material needs archival;
14. final Ansible/bootstrap repository layout;
15. final machine-name/color registry update when the cutover occurs.

---

## RETIRE candidates

Do not delete anything until an explicit pre-cutover review proves it disposable.

Likely RETIRE/RECREATE items include:

- Docker build cache
- transient CI workspaces
- superseded Docker images
- obsolete Secure Agent Gateway release/rollback directories whose revisions are preserved in Git
- stale Crochet candidate/worktree directories once unique commits/data are proven durable
- old Arcana pre-Git backup material after explicit archival verification

---

## Planned deployment stack

The target recovery design is:

```text
Ubuntu installer
    |
    v
SATA RAID1 OS
    |
    v
small server1 bootstrap
    |
    v
Ansible / repository-owned host configuration
    |
    +--> storage mounts
    +--> users/groups
    +--> networking/firewall
    +--> Docker/containerd
    +--> PostgreSQL
    +--> Secure Agent Gateway / CI
    +--> Arcana services
    +--> Crochet services
    +--> monitoring
    |
    v
secure secret restore
    |
    v
durable state restore
    |
    v
PostgreSQL logical restore
    |
    v
acceptance validation
```

The deployment stack must be idempotent where practical and distinguish:

- configuration/code: Git
- secrets/credentials: secure restore package
- durable application/CI/database data: backup/restore
- disposable state: recreate

---

## Acceptance criteria

A rebuilt server1 is not complete merely because systemd units or containers start.

Final acceptance must include at minimum:

- correct hostname and host identity
- healthy SATA RAID1
- healthy four-NVMe RAID10
- single-NVMe fast tier mounted correctly
- expected filesystems mounted by stable identifiers
- PostgreSQL 18 healthy on RAID10
- expected databases/roles restored
- database clients can connect only from approved paths
- Docker/containerd uses the fast tier as designed
- Gateway CI controller/coordinator/helper/registration/telemetry healthy
- Arcana validation controller/coordinator/helper/registration healthy
- Crochet validation controller/coordinator/helper/registration healthy
- Arcana/Crochet release-preflight paths healthy
- application secrets present with exact intended permissions
- SSH/deploy/source-fetch identities functional
- intended firewall/listener policy enforced
- monitoring sees server1 correctly
- at least one real end-to-end self-hosted validation job succeeds
- independent backup/restore validation completed for PostgreSQL and critical durable state

---

## Cutover principle

Do not erase or repurpose the current Samsung 970 EVO root NVMe until:

1. server1 boots independently from the SATA RAID1;
2. the RAID10 is attached and validated;
3. PostgreSQL and durable CI state are restored and validated;
4. application and CI services pass acceptance;
5. required secrets/identities are proven present;
6. rollback no longer depends on the old NVMe installation.

Only then should the Samsung NVMe be reformatted for the high-churn `/srv/fast` tier.

---

## Evidence state

Completed:

- broad read-only live-host inventory
- protected read-only inventory with sudo
- PostgreSQL logical inventory
- protected credential metadata inventory
- Docker bind-mount/network inventory
- current host policy inventory
- current CI durable-state inventory

Pending:

- secure backup package definition
- pre-cutover data backup
- deployment-stack implementation
- storage/filesystem implementation
- cutover execution
- final acceptance evidence
