# server1 Deployment Stack

This directory contains the repository-owned rebuild/bootstrap stack for 🟥 `server1`.

The stack is intentionally phased. Storage, database restore, protected credentials, and application activation are not allowed to run until their prerequisites have been validated.

## Recovery phases

1. `00-base` — establish the base Ubuntu host, core packages, timezone, and required directory roots.
2. `10-storage` — validate/mount the single-NVMe fast tier and four-NVMe RAID10 durable tier. This phase is not implemented until final device/filesystem identities are known after hardware installation.
3. `20-secrets` — restore protected credentials from the encrypted recovery bundle. Secret material is never stored in Git.
4. `30-postgresql` — install PostgreSQL 18, place durable data on RAID10, restore roles/databases, and apply reviewed network policy.
5. `40-gateway-ci` — recreate Gateway/Arcana/Crochet self-hosted CI from reviewed Secure Agent Gateway sources and restore coordinator state.
6. `50-applications` — restore Arcana/Crochet application checkouts and container services.
7. `60-monitoring` — restore host metrics and dashboard-facing telemetry.
8. `90-acceptance` — prove the rebuilt host meets the SERVER1-REBUILD-MANIFEST acceptance criteria.

## Source of truth

- Rebuild requirements: `docs/SERVER1-REBUILD-MANIFEST.md`
- OS-install gate/checklist: `docs/SERVER1-OS-INSTALL-CHECKLIST.md`
- Shared operating rules: `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

## Bootstrap model

The initial post-install run is local on `server1`; it does not require DNS or a remote Ansible controller. The installer-created administrator account is expected to be `ryesel` and the installer hostname is expected to be `server1`.

Do not attach/reformat the preserved Samsung 970 EVO until the rebuild manifest's rollback conditions have been satisfied.
