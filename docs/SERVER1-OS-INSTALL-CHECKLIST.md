# server1 OS Installation Checklist

**Source host:** 🟩 `ubuntu-cpu-llm`  
**Target host:** 🟥 `server1`

## Hard pre-install gates

Do not remove the original Samsung 970 EVO or begin the SATA RAID1 OS installation until all of the following are PASS:

- encrypted PostgreSQL recovery archive created;
- encrypted protected-state/secrets archive created;
- both recovery archives copied off-host and SHA-256 verified;
- current Gateway/Arcana/Crochet CI coordinator state captured consistently;
- repository-owned server1 deployment/bootstrap files committed;
- final cutover-readiness check returns `SERVER1_CUTOVER_READINESS=PASS`.

## Hardware state for installation

At OS installation time:

- power the machine completely off;
- physically remove the existing Samsung 970 EVO 1 TB NVMe and store it safely;
- do not connect the four-NVMe durable RAID10 during the OS installation;
- connect only the two new SATA SSDs intended for the OS RAID1 plus the Ubuntu installer media.

The Samsung NVMe remains the cold rollback copy of the current `ubuntu-cpu-llm` installation until server1 acceptance is complete.

## Ubuntu install choices

- Install Ubuntu Server 26.04 LTS/26.04.1 media matching the current host generation.
- Use the installer's **Custom storage layout**.
- Create Linux software RAID (MD) RAID1 from the two SATA devices/partitions for the OS data area.
- Ensure both physical SATA drives receive the required UEFI boot/EFI treatment so the system can later be proven bootable with either RAID1 member unavailable.
- Do not select or initialize any NVMe device.
- Hostname: `server1`.
- Administrator username: `ryesel`.
- Timezone after bootstrap: `America/Denver`.
- Wired networking may remain DHCP initially; the permanent address/DNS policy is applied only after the rebuilt host is reachable and reviewed.
- Install/enable OpenSSH Server.
- Do not install optional featured server snaps unless specifically required.

## Immediate post-install gate

Before attaching any preserved storage:

1. prove `server1` boots from the SATA RAID1;
2. verify RAID1 is healthy;
3. install the repository-owned bootstrap dependencies;
4. apply the `00-base` playbook;
5. verify SSH and networking;
6. shut down and test independent bootability from each SATA member before proceeding.

Only after those checks pass should the four-NVMe RAID10 be attached for read-only identification/assembly validation.

Do not reconnect/reformat the Samsung 970 EVO until PostgreSQL, CI state, application services, and full server acceptance have passed.
