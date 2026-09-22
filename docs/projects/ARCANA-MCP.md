# Arcana MCP Project Scope

**Status:** AUTHORITATIVE  
**Repository:** `ry-arcana-blade/architecture`  
**Branch:** `main`  
**Path:** `docs/projects/ARCANA-MCP.md`

## Purpose

This document defines the workstream boundary, recovery behavior, repository ownership, and cross-project rules for the **Arcana MCP** ChatGPT Project.

The Project is the authoritative workspace for Arcana MCP, Secure Agent Gateway, MCP gateway infrastructure, orchestration infrastructure directly supporting MCP work, and the deployment, testing, diagnostics, and hardening of those systems.

This document supplements the common operating instructions at:

`docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The common operating instructions remain authoritative for shell safety, machine identities, command delivery, validation, hostname guards, classifications, sudo handling, rollback evidence, and output-return procedures.

---

## 1. Hard Project Boundary

**PROJECT SCOPE IS A HARD WORKSTREAM BOUNDARY.**

When operating inside the Arcana MCP Project, do not silently transition into another Arcana workstream merely because that workstream is related, recent, visible in memory, referenced by a dependency, or has unfinished roadmap items.

In particular:

- Do not select Arcana Blade Web application roadmap work as the active task.
- Do not resume Arcana Web AI-authoring, catalogue, gameplay, UI, application-feature, or application-roadmap work unless the user explicitly asks to cross into that workstream.
- Do not use recency alone to choose between MCP work and Arcana Web work.
- Do not treat an open Arcana Web pull request as ownership of the current task merely because MCP infrastructure is waiting on or observing it.
- Do not reinterpret a dependency as the active workstream.

Cross-project information may be inspected when necessary, but it remains **REFERENCE OR DEPENDENCY CONTEXT** unless the user explicitly transfers the task.

---

## 2. Recovery Semantics

When the user says any of the following, or words with the same intent:

- "recover"
- "recover and continue"
- "continue"
- "resume"
- "pick up where we left off"
- "continue work"
- "where were we?"
- "keep going"

interpret the request relative to the **Arcana MCP Project only**.

Recovery priority:

1. Recover the most recent active Arcana MCP task from conversations in this Project.
2. Recover the durable repository state associated with that MCP task.
3. Inspect authoritative MCP task/status documents when applicable.
4. Inspect open MCP-related pull requests, CI, gateway jobs, deployment state, or wake/orchestration state when relevant.
5. Continue that same task if it remains active and coherent.
6. If no active MCP task can be recovered, state that clearly rather than selecting work from another Project.

Do not fall back to the Arcana Web roadmap merely because no obvious MCP task is found.

---

## 3. Primary Workstream Ownership

The Arcana MCP Project owns work involving:

- Arcana MCP
- Secure Agent Gateway
- MCP gateway behavior
- MCP surface design and validation
- Gateway identity and capability validation
- Gateway deployment and rollout
- Gateway health, security, diagnostics, and operational hardening
- MCP-related server integration
- MCP-related service management
- MCP orchestration infrastructure
- event-driven MCP wake/resume infrastructure
- CI-to-orchestrator signaling used to support MCP development workflows
- deployment readiness and recovery behavior for MCP/gateway services
- Crochet gateway infrastructure when the subject is MCP/gateway operation rather than Crochet application/product work
- common architecture work when directly required to complete an MCP objective

---

## 4. Repository Context

Repositories commonly associated with this Project include, as applicable:

- `ry-arcana-blade/secure-agent-gateway`
- `ry-arcana-blade/arcana-work-wake`
- `ry-arcana-blade/architecture` for shared operating policy and architecture documentation

Other repositories may be inspected when they are dependencies of an MCP objective.

The presence of a repository in dependency context does not automatically make its roadmap the active roadmap for this Project.

---

## 5. Arcana Blade Web Boundary

The repository:

`ry-arcana-blade/arcana-blade-web`

is primarily owned by the Arcana Web workstream.

Inside Arcana MCP, it may be inspected for dependency state such as:

- pull-request status
- CI completion
- staging or evaluation state
- integration compatibility
- orchestration signals
- exact revisions required by an MCP rollout
- evidence needed to verify an MCP dependency

However, unless the user explicitly requests otherwise, do not:

- select the next Arcana Web roadmap item
- begin Arcana Web application development
- advance AI-authoring feature work
- modify application behavior solely because Arcana Web has unfinished work
- interpret Arcana Web `CURRENT_WORK.md` as the active MCP task
- convert an MCP recovery request into an Arcana Web development session

A dependency may block MCP work. If so, report the dependency and remain within the MCP workstream.

---

## 6. Active Task Selection

If an MCP task is already active, recover and continue it instead of inventing a competing task.

If there is no active MCP task and the user has asked for new work:

1. inspect the authoritative MCP repository state;
2. inspect relevant MCP roadmap/current-work documents;
3. inspect open MCP-related pull requests and CI;
4. select only a coherent unfinished MCP objective;
5. keep the objective bounded.

Do not use another Project's roadmap to fill an MCP task-selection vacuum.

---

## 7. Cross-Project Dependencies

Cross-project inspection is allowed when necessary to answer questions such as:

- Is an Arcana Web PR blocking an MCP rollout?
- Did application CI emit the signal the MCP orchestrator expects?
- Is the deployment revision expected by the Gateway available?
- Did a dependent service change its contract?
- Does Architecture contain a newer shared operating rule?

When cross-project information is used:

- identify it as dependency/reference context;
- keep task ownership with Arcana MCP;
- do not continue unrelated work in the referenced Project;
- return to the MCP objective after collecting the needed evidence.

If completing the task truly requires transferring ownership into another Project, make that transition explicit rather than silently doing it.

---

## 8. Orchestration and Wake Work

When working on event-driven orchestration, wake relays, CI terminal signaling, or resume behavior:

- distinguish the wake/coordination repository from the development repository;
- treat wake comments and relay events as signals, not execution authority;
- preserve the reviewed repository/task identity embedded in the orchestration design;
- do not treat an orchestration dependency on Arcana Web as permission to select Arcana Web roadmap work;
- keep recovery anchored to the MCP/orchestration task that caused the dependency to be inspected.

---

## 9. Common Operating Instructions

Before generating shell commands, deployment instructions, diagnostics, administrative actions, security changes, service restarts, or downloadable shell scripts, retrieve and follow:

Repository: `ry-arcana-blade/architecture`  
Branch: `main`  
Path: `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The machine/color registry currently includes:

- 🟦 `server0`
- 🟩 `ubuntu-cpu-llm`
- 🟧 `monitoring-kiosk`
- 🟪 `EVO-X2`

Do not duplicate or override the common shell-safety policy here unless an MCP-specific rule genuinely requires it.

---

## 10. Failure to Recover

If the user asks to recover or continue and no trustworthy active Arcana MCP task can be established:

- do not guess;
- do not choose Arcana Web work;
- do not select a task merely because it is the newest visible Arcana task;
- report that no active MCP task was recovered;
- identify the MCP evidence that was checked, when useful.

If partial MCP context exists, prefer continuing from the strongest durable evidence rather than importing a different Project's workstream.

---

## 11. Scope Changes

The user may explicitly direct work across Project boundaries.

Examples include:

- asking Arcana MCP to inspect or modify Arcana Web for a specific integration task;
- explicitly transferring a task from Arcana Web into Arcana MCP;
- requesting a cross-project architecture change;
- requesting coordinated changes spanning multiple repositories.

Such an explicit instruction may temporarily broaden scope for that task. Do not treat that temporary scope expansion as a permanent ownership change unless the user says so.

---

## 12. Governing Principle

Inside the Arcana MCP Project:

**Recover MCP work as MCP work. Use other Arcana projects as dependencies when necessary, not as automatic continuation targets.**
