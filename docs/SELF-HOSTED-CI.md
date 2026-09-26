# Self-Hosted CI Operating Guide

**Status:** AUTHORITATIVE INTERNAL OPERATING NOTE  
**Primary validation host:** `ubuntu-cpu-llm`  
**Shared implementation owner:** `ry-arcana-blade/secure-agent-gateway`

## Purpose

This document records how Arcana projects should consume the reviewed self-hosted CI system. It is an operating guide for development conversations and project workers; it does not widen the authority of the CI runtime.

The fixed executors, coordinator, helper, registration/status publisher, service units, security boundaries, and deployment procedures remain owned by the Secure Agent Gateway repository and its reviewed source.

## Arcana Web validation identity

For `ry-arcana-blade/arcana-blade-web`:

- workflow identity: `arcana_ci_full`
- GitHub commit-status context: `arcana/self-hosted-validation`
- validation host: `ubuntu-cpu-llm`
- admission identity: exact lowercase 40-hex Git revision
- caller-selectable repository, command, path, image, environment, network, database, credential, and timeout authority: none

The current reviewed Arcana executor reproduces the repository's broad PR validation path, including static/contract checks, validation-batch checks, image provenance, collectstatic, PostgreSQL-backed Django checks, migration-plan validation, focused PostgreSQL tests, and the full PostgreSQL suite when the reviewed batch contract selects full mode.

## Development lifecycle use

For Arcana Web work, use self-hosted CI as the normal exact-revision broad-validation evidence when the fixed Arcana path is available.

1. Establish the exact PR head revision.
2. Require `arcana/self-hosted-validation` to reach `success` for that same exact revision before treating broad CI as passed.
3. Do not reuse success from an older revision after any source or documentation commit changes the PR head.
4. Keep focused tests and mutable-batch gates before freeze; self-hosted broad validation does not replace slice-focused validation.
5. After a frozen-head self-hosted pass, continue the normal staging update -> staging test -> applicable persistent evaluation -> staging reset -> final review/mergeability lifecycle.
6. A failed self-hosted result is evidence to diagnose; do not bypass it by relying on an older hosted result.
7. Hosted GitHub Actions may remain enabled while migration is in progress, but its presence does not change the exact-revision requirement for the self-hosted status when this guide says to use the self-hosted path.

## Campaign use

For the Arcana AI refinement campaign, campaign authority remains defined by the protected-main campaign contract and issue. Self-hosted CI changes only where broad validation executes; it does not weaken campaign scope, review, staging, evaluation, reset, or merge gates.

A campaign PR that changes head after a successful self-hosted run must obtain a fresh `arcana/self-hosted-validation=success` on the new exact head.

## Evidence and monitoring

Use GitHub's exact-revision commit status as the durable external success/failure signal. Operational telemetry may be used to monitor active progress, but telemetry is not a substitute for terminal exact-revision status.

For the Arcana path, accepted live qualification proved pending -> success publication for `arcana/self-hosted-validation` and cleanup of validation containers, PostgreSQL peer, private network, temporary source/fetch material, and helper instance.

## Security boundary

The Arcana validation path is deliberately fixed and isolated. Candidate code runs non-root in the reviewed validation container with read-only source, no Docker socket, no host secrets, no host network, dropped capabilities, no-new-privileges, bounded resources, and a disposable PostgreSQL peer on an internal run-scoped network.

Do not replace the fixed path with ad-hoc shell execution merely for convenience.

## Cross-project reuse

The shared design is intended to support additional reviewed project-specific fixed validation stacks. Each project must retain its own repository/status identity and bounded executor authority rather than turning the coordinator into a generic caller-programmable CI service.

## Source of truth

For shell, deployment, diagnostic, restart, or server-administration work, also follow:

- `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

For Arcana Web project recovery and workstream boundaries, also follow:

- `docs/projects/ARCANA-WEB.md`

Implementation details and runtime deployment procedures remain authoritative in `ry-arcana-blade/secure-agent-gateway`.
