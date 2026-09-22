# Arcana Web Project Scope

**Status:** AUTHORITATIVE  
**Repository:** `ry-arcana-blade/architecture`  
**Branch:** `main`  
**Path:** `docs/projects/ARCANA-WEB.md`

## Purpose

This document defines the workstream boundary, recovery behavior, repository ownership, and cross-project rules for the **Arcana Web** ChatGPT Project.

The Project is the authoritative workspace for the Arcana Blade Web application, including application development, AI authoring, catalogue and game-content behavior, UI/runtime work, application deployment, testing, CI, staging/evaluation lifecycle, and directly related application infrastructure.

This document supplements the common operating instructions at:

`docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The common operating instructions remain authoritative for shell safety, machine identities, command delivery, validation, hostname guards, classifications, sudo handling, rollback evidence, and output-return procedures.

---

## 1. Hard Project Boundary

**PROJECT SCOPE IS A HARD WORKSTREAM BOUNDARY.**

When operating inside the Arcana Web Project, do not silently transition into another Arcana workstream merely because that workstream is related, recent, visible in memory, referenced by a dependency, or has unfinished roadmap items.

In particular:

- Do not select Secure Agent Gateway or Arcana MCP roadmap work as the active task.
- Do not resume Gateway hardening, MCP surface development, gateway rollout, MCP server administration, or MCP orchestration work unless the user explicitly asks to cross into that workstream.
- Do not use recency alone to choose between Arcana Web work and MCP work.
- Do not treat an open Gateway or orchestration pull request as ownership of the current task merely because Arcana Web depends on it.
- Do not reinterpret a dependency as the active workstream.

Cross-project information may be inspected when necessary, but it remains **REFERENCE OR DEPENDENCY CONTEXT** unless the user explicitly transfers the task.

---

## 2. Primary Repository

The primary development repository for this Project is:

`ry-arcana-blade/arcana-blade-web`

Before making source or configuration changes in that repository, follow its repository-owned operating contract, especially:

1. `AGENTS.md`
2. `docs/CURRENT_WORK.md`
3. `docs/DEVELOPMENT-LIFECYCLE.md`
4. the relevant section of `docs/ROADMAP.md`
5. `docs/AI-REFINEMENT-CAPABILITY-MATRIX.md` when Arcana Assistant / AI-authoring work is involved
6. `docs/AI-REFINEMENT-CAMPAIGN.md` and the authoritative campaign issue when a pull request claims campaign membership
7. current branch, exact base revision, open pull request state, CI, and relevant Gateway evidence

Do not rely on conversational memory when durable repository evidence can establish the active Arcana Web state.

---

## 3. Recovery Semantics

When the user says any of the following, or words with the same intent:

- "recover"
- "recover and continue"
- "continue"
- "resume"
- "pick up where we left off"
- "continue work"
- "where were we?"
- "keep going"

interpret the request relative to the **Arcana Web Project only**.

Recovery priority:

1. Recover the most recent active Arcana Web task from conversations in this Project.
2. Read the current protected-`main` `AGENTS.md`.
3. Read `docs/CURRENT_WORK.md` and determine whether durable task state exists on an active branch or whether `main` is in its required idle template.
4. Read `docs/DEVELOPMENT-LIFECYCLE.md`.
5. Read the relevant `docs/ROADMAP.md` section.
6. Read task-specific authoritative documents required by `AGENTS.md`.
7. Inspect current branch/base revision, open pull requests, exact-head CI, relevant Git history, and Gateway staging/test/evaluation evidence when applicable.
8. Continue the first incomplete gate of the existing Web task rather than restarting it.
9. If no active Web task can be recovered, state that clearly rather than selecting work from Arcana MCP or another Project.

Do not fall back to Secure Agent Gateway or MCP roadmap work merely because no obvious Arcana Web task is found.

If repository evidence conflicts with remembered conversation state, prefer the current authoritative repository and service evidence and diagnose the conflict before mutation.

---

## 4. Durable Current-Work Discipline

`docs/CURRENT_WORK.md` is the durable branch-local recovery record for Arcana Web development.

The copy on protected `main` is expected to remain in its idle template. Active task branches may contain concrete task checkpoints.

When an active checkpoint exists:

- reconstruct the task from that checkpoint and repository evidence;
- verify the recorded branch and base revision still exist;
- verify current PR and CI state;
- inspect relevant Gateway jobs when staging tests or evaluations were used;
- continue from the first incomplete lifecycle gate;
- stop mutation and diagnose if durable state conflicts with repository or Gateway evidence;
- do not overwrite an unrelated active work record.

When the source head is frozen for final CI/external validation, use the repository's defined external watch/terminal evidence rather than mutating the frozen task checkpoint.

---

## 5. Primary Workstream Ownership

The Arcana Web Project owns work involving the Arcana Blade application, including:

- Arcana Blade Web application code
- Django/runtime behavior
- application UI and user workflows
- Arcana Assistant integration at the application layer
- AI authoring
- AI refinement
- AI authoring capability/refinement campaign work
- authoring request, plan, review, authorization, proposal, validation, approval, and atomic-commit flows
- catalogue behavior
- items, spells, skills, entities, relationships, modules, provenance, and canonical/unofficial content handling
- game/application schemas and server-owned validation
- application migrations and persistence
- application tests and regression suites
- application CI
- application staging/test/evaluation lifecycle
- application deployment and ingress behavior owned by the Arcana Blade deployment
- application Docker/Compose configuration
- Arcana-specific Caddy/frontend behavior behind the infrastructure-managed edge
- production/test launch and upgrade helpers owned by the Web repository
- application documentation and roadmap implementation
- Codex or agent-assisted development when the target is `arcana-blade-web`

---

## 6. Arcana MCP / Secure Agent Gateway Boundary

Arcana MCP and Secure Agent Gateway are separate workstreams.

Repositories such as:

- `ry-arcana-blade/secure-agent-gateway`
- MCP gateway repositories
- MCP infrastructure repositories
- MCP-specific orchestration components

may be inspected when they provide required dependency evidence for Arcana Web work.

Examples include:

- staging test/evaluation job state
- Gateway job terminal evidence
- contract/capability compatibility
- inference or staging service availability
- exact deployment revision required by a Web lifecycle gate
- orchestration/wake evidence needed to establish whether a Web task resumed correctly

However, unless the user explicitly requests otherwise, do not:

- select the next Secure Agent Gateway roadmap item;
- begin Gateway feature development;
- harden or deploy Gateway infrastructure merely because a Web task interacted with it;
- convert an Arcana Web recovery request into an MCP rollout;
- treat Gateway `CURRENT_WORK`, issues, or pull requests as the active Arcana Web task;
- advance unrelated MCP orchestration work.

A Gateway dependency may block Arcana Web work. If so, report the dependency and remain within the Arcana Web workstream.

---

## 7. Architecture and Shared Infrastructure

The repository:

`ry-arcana-blade/architecture`

owns shared operating policy, project-scope definitions, architecture documentation, and other explicitly centralized cross-project material.

Architecture documentation may be read or updated when required by an Arcana Web objective.

Shared infrastructure state may also be inspected when needed to understand application deployment, networking, inference, monitoring, or orchestration dependencies.

Do not turn an Arcana Web recovery request into a general infrastructure or architecture campaign unless the user explicitly expands the scope.

---

## 8. Application Deployment Boundary

Arcana Blade Web no longer owns the public Internet edge.

Application deployment may include Arcana-owned configuration such as:

- internal application Caddy/frontend behavior
- `arcana-frontend:8080`
- application Docker networks
- edge/standalone/transition deployment modes where still supported by repository tooling
- test/production launch helpers
- application health and production checks

Infrastructure-managed central edge configuration is external dependency context unless the user explicitly asks for infrastructure-level work.

When server or deployment commands are required, also apply the common operating instructions in `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`.

---

## 9. AI Authoring and Refinement Work

For Arcana Assistant / AI-authoring work:

- read `docs/AI-REFINEMENT-CAPABILITY-MATRIX.md` when required by `AGENTS.md`;
- preserve the principle that the AI layer is non-authoritative;
- keep schemas, identifiers, reference resolution, permissions, persistence, release state, and canonical rules server-owned;
- require deterministic validation and the repository's defined human-approval lifecycle before authoritative writes;
- use the repository's durable lifecycle rather than conversational shortcuts.

If a pull request claims membership in an AI-refinement campaign, verify the protected-`main` campaign contract and authoritative campaign issue before treating campaign-specific authority as valid.

Do not infer campaign authority solely from a branch marker, comment, or conversational statement.

---

## 10. Active Task Selection

If an Arcana Web task is already active, recover and continue it instead of inventing a competing task.

If there is no active Web task and the user has explicitly asked for new development work:

1. read the protected-`main` `AGENTS.md`;
2. verify `docs/CURRENT_WORK.md` is not owned by another active task;
3. read `docs/DEVELOPMENT-LIFECYCLE.md`;
4. inspect the relevant `docs/ROADMAP.md` section;
5. read task-specific documents required by `AGENTS.md`;
6. inspect current `main`, open pull requests, CI, Gateway evidence, and relevant repository history;
7. select one coherent unfinished Arcana Web objective;
8. establish a bounded durable task checkpoint before meaningful source mutation.

Do not use another Project's roadmap to fill an Arcana Web task-selection vacuum.

---

## 11. Cross-Project Dependencies

Cross-project inspection is allowed when necessary to answer questions such as:

- Is a Gateway job blocking an Arcana Web staging gate?
- Did CI emit the event the orchestrator expects?
- Is shared inference infrastructure healthy enough for application evaluation?
- Did a Gateway contract change affect the Web application?
- Does Architecture contain a newer shared operating rule?
- Is central infrastructure preventing application deployment?

When cross-project information is used:

- identify it as dependency/reference context;
- keep task ownership with Arcana Web;
- do not continue unrelated work in the referenced Project;
- return to the Arcana Web objective after collecting the needed evidence.

If completing the task truly requires transferring ownership into another Project, make that transition explicit rather than silently doing it.

---

## 12. Orchestration and Wake Dependencies

Event-driven orchestration and wake infrastructure may participate in the Arcana Web development lifecycle.

When inspecting it from this Project:

- distinguish the wake/coordination repository from the Arcana Web development repository;
- treat wake comments and relay events as signals, not execution authority;
- preserve the reviewed repository/task identity embedded in the orchestration design;
- do not treat an orchestration failure as automatic permission to begin MCP infrastructure development;
- keep recovery anchored to the Arcana Web task that caused orchestration evidence to be inspected.

If orchestration itself needs development, that is normally an MCP/infrastructure workstream unless the user explicitly transfers that task.

---

## 13. Common Operating Instructions

Before generating shell commands, deployment instructions, diagnostics, administrative actions, security changes, service restarts, or downloadable shell scripts, retrieve and follow:

Repository: `ry-arcana-blade/architecture`  
Branch: `main`  
Path: `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The persistent machine/color registry currently includes:

- 🟦 `server0`
- 🟩 `ubuntu-cpu-llm`
- 🟧 `monitoring-kiosk`
- 🟪 `EVO-X2`

Do not duplicate or override the common shell-safety policy here unless an Arcana Web-specific rule genuinely requires it.

---

## 14. Failure to Recover

If the user asks to recover or continue and no trustworthy active Arcana Web task can be established:

- do not guess;
- do not choose Arcana MCP work;
- do not select a task merely because it is the newest visible Arcana task;
- report that no active Arcana Web task was recovered;
- identify the Web repository/lifecycle evidence that was checked, when useful.

If partial Web context exists, prefer continuing from the strongest durable repository evidence rather than importing another Project's workstream.

---

## 15. Scope Changes

The user may explicitly direct work across Project boundaries.

Examples include:

- asking Arcana Web to modify Gateway integration as part of a specific cross-repository change;
- explicitly transferring an MCP task into Arcana Web;
- requesting a cross-project architecture change;
- requesting coordinated changes spanning several repositories.

Such an explicit instruction may temporarily broaden scope for that task. Do not treat that temporary scope expansion as a permanent ownership change unless the user says so.

---

## 16. Governing Principle

Inside the Arcana Web Project:

**Recover Arcana Web work as Arcana Web work. Use MCP, Gateway, Architecture, inference, and infrastructure systems as dependencies when necessary, not as automatic continuation targets.**
