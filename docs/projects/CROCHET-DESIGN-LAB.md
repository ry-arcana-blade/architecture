# Crochet Design Lab Project Scope

**Status:** AUTHORITATIVE  
**Repository:** `ry-arcana-blade/architecture`  
**Branch:** `main`  
**Path:** `docs/projects/CROCHET-DESIGN-LAB.md`

## Purpose

This document defines the workstream boundary, recovery behavior, repository ownership, MCP authority, and cross-project rules for the **Crochet Design Lab** ChatGPT Project.

The Project is the authoritative workspace for Crochet Design Lab application development, deterministic crochet modeling, source/corpus work, planner/evaluation work, project-specific inference integration, deployment, testing, qualification, and MCP work that directly serves Crochet Design Lab.

This document supplements the common operating instructions at:

`docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The common operating instructions remain authoritative for shell safety, machine identities, command delivery, validation, hostname guards, classifications, sudo handling, rollback evidence, and output-return procedures.

---

## 1. Hard Project Boundary

**PROJECT SCOPE IS A HARD WORKSTREAM BOUNDARY.**

When operating inside the Crochet Design Lab Project, do not silently transition into Arcana Web, general Arcana MCP, Secure Agent Gateway platform work, or another project merely because that work is related, recent, visible in memory, referenced by a dependency, or unfinished.

In particular:

- Do not select Arcana Web roadmap or application work as the active task.
- Do not select unrelated Arcana MCP / Secure Agent Gateway roadmap work as the active task.
- Do not use recency alone to choose another Arcana project over Crochet work.
- Do not reinterpret an external dependency as the active workstream.
- Do not treat a shared Gateway pull request as ownership of the current task unless it is directly required by the active Crochet objective.

Cross-project information may be inspected when necessary, but it remains **REFERENCE OR DEPENDENCY CONTEXT** unless the user explicitly transfers the task.

---

## 2. Primary Repository and Canonical Recovery Source

The primary repository for this Project is:

`ry-arcana-blade/crochet-design-lab`

The canonical project handoff is:

`docs/PROJECT_STATE.md`

The repository README explicitly directs recovery to that document. Treat it as the first durable source for the validated baseline, active work, source/corpus state, and exact next steps.

The project roadmap is:

`docs/ROADMAP.md`

When recovering development state, prefer current repository evidence over remembered conversational state.

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

interpret the request relative to the **Crochet Design Lab Project only**.

Recovery priority:

1. Recover the most recent active Crochet task from conversations in this Project.
2. Read the current `docs/PROJECT_STATE.md`.
3. Read the relevant section of `docs/ROADMAP.md`.
4. Inspect current `main`, relevant branches, open pull requests, CI, release/qualification evidence, and recent repository history.
5. Inspect relevant Secure Agent Gateway / MCP evidence when Crochet used Gateway jobs, exact-revision qualification, inference/evaluation, or MCP publication.
6. Continue the first incomplete Crochet task or gate supported by durable evidence.
7. If no active Crochet task can be recovered, state that clearly rather than selecting work from Arcana Web or general Arcana MCP.

Do not restart completed work merely because an older conversation is easier to recover.

If repository evidence conflicts with conversational memory, prefer the current authoritative repository and service evidence and diagnose the mismatch before mutation.

---

## 4. Primary Workstream Ownership

The Crochet Design Lab Project owns work involving:

- `ry-arcana-blade/crochet-design-lab`
- Crochet IR
- stitch graph contracts
- deterministic crochet construction
- source-gated structural semantics
- planner/lowering behavior
- topology and arithmetic validation
- source/corpus provenance
- calibration evidence
- project-specific inference and evaluation
- human acceptance workflows
- Crochet application/API behavior
- Crochet testing and CI
- Crochet staging, qualification, and deployment
- Crochet-specific Gateway integration
- Crochet-specific MCP surfaces, tools, contracts, adapters, handlers, tests, and publication behavior
- Crochet-specific orchestration required to exercise or validate those MCP surfaces
- documentation and roadmap work for the Crochet product

Provider/model output remains evidence rather than authority. Deterministic lowering, validation, exact source quantities, and explicit human acceptance remain authoritative according to the Crochet repository's own contracts.

---

## 5. Crochet Is Allowed to Own Its MCP Work

Unlike Arcana Web, this Project is explicitly allowed to perform **its own MCP work when needed to accomplish a Crochet objective**.

Project-owned MCP work may include:

- defining or extending Crochet-specific MCP tools;
- changing Crochet-specific MCP request or response schemas;
- implementing Crochet-specific MCP handlers or adapters;
- adding project-specific Gateway target/profile/capability configuration;
- adding Crochet-specific health, validation, diagnostics, or tests;
- changing Crochet-specific MCP publication or discovery behavior;
- adding or modifying Crochet-specific inference/evaluation operations exposed through MCP;
- fixing Crochet-specific Gateway integration failures;
- adding exact-revision qualification support required by Crochet;
- updating Crochet-specific orchestration or workflow glue needed to execute these capabilities;
- changing shared Gateway code when the change is narrowly required for Crochet and remains safely bounded.

The existence of MCP work does **not** automatically transfer the task to the Arcana MCP Project.

If the active objective is Crochet-owned, the Crochet Design Lab Project may carry the necessary MCP work through completion.

---

## 6. Shared Gateway Boundary

The shared Secure Agent Gateway remains a cross-project platform dependency.

Crochet may modify shared Gateway code when all of the following are true:

1. the change is directly required by a concrete Crochet objective;
2. the scope is bounded and reviewable;
3. the change does not silently broaden authority for unrelated projects;
4. generic/shared behavior is preserved unless the task explicitly requires a reviewed shared change;
5. relevant Gateway tests and contract checks are run;
6. the Crochet task remains the reason for the change.

Examples of in-scope shared Gateway work:

- adding a Crochet-only target alias or operation;
- fixing a bug that prevents a Crochet operation from executing;
- adding a capability check required by a Crochet contract;
- extending a shared primitive in a backward-compatible way because Crochet requires it;
- adding Crochet-specific validation or test coverage to the Gateway.

Examples that normally belong to Arcana MCP unless explicitly requested here:

- broad Gateway architecture redesign;
- unrelated Arcana target/profile work;
- platform-wide authentication or authorization redesign;
- generic orchestration campaigns unrelated to Crochet;
- MCP work whose primary beneficiary and objective are not Crochet;
- sweeping shared infrastructure refactors undertaken only because Crochet happened to expose the issue.

If a Crochet task reveals a broad shared-platform problem, it is acceptable to diagnose and document it here. Keep the active workstream Crochet-owned unless the user explicitly chooses to transfer the broader platform task.

---

## 7. MCP Contract Discipline

Crochet MCP work must preserve explicit contract boundaries.

When changing a public MCP surface:

- treat schema changes as versioned contract/publication changes rather than silently expanding behavior;
- preserve deterministic validation;
- add regression coverage for cross-workflow isolation;
- protect secrets and private provider details;
- keep mutation authority explicit;
- fail closed on unsupported or ambiguous semantics;
- do not allow provider output to bypass deterministic Crochet acceptance rules.

Project-specific MCP convenience must not weaken the authoritative Crochet model.

---

## 8. Secure Agent Gateway Qualification

Crochet may use Secure Agent Gateway as part of its normal development and qualification lifecycle.

Relevant Gateway evidence may include:

- `crochet_engine_tests`
- `crochet_ci_full`
- exact-revision admission/qualification
- Crochet-specific inference/evaluation jobs
- terminal Gateway job evidence
- target/profile/capability checks
- deployment/readiness evidence

When recovering a Crochet task that used Gateway qualification:

- verify the exact repository revision associated with the evidence;
- distinguish historical validated milestones from current `main`;
- do not invent missing job IDs or hashes;
- do not treat successful Gateway execution as authority to accept a Crochet proposal;
- continue to require the project-defined deterministic and human acceptance gates.

---

## 9. Arcana MCP Boundary

The **Arcana MCP Project** owns general/shared MCP platform work that is not primarily a Crochet objective.

Inside Crochet Design Lab:

- Arcana MCP documentation and repositories may be consulted as dependency context;
- shared Gateway code may be changed when narrowly required by Crochet, as defined above;
- unrelated MCP roadmap work must not be selected during Crochet recovery;
- an open MCP task elsewhere does not displace an active Crochet task;
- a Crochet-owned MCP change does not need to be moved to the Arcana MCP Project merely because it touches MCP.

The deciding question is **task ownership**, not technology category.

If the objective is "make Crochet capability X work through MCP," that is Crochet work.

If the objective is "redesign the shared MCP platform for all projects," that is normally Arcana MCP work unless the user explicitly assigns it here.

---

## 10. Arcana Web Boundary

Arcana Web is a separate application workstream.

Arcana Web may be consulted when a shared infrastructure or integration dependency genuinely affects Crochet, but do not:

- select Arcana Web roadmap items;
- resume Arcana Web AI-authoring work;
- adopt Arcana Web `CURRENT_WORK.md`;
- convert a Crochet recovery request into Arcana Web development.

---

## 11. Architecture and Shared Operating Policy

The repository:

`ry-arcana-blade/architecture`

owns shared operating policy, project-scope definitions, and centralized architecture documentation.

Before generating shell commands, deployment instructions, diagnostics, administrative actions, security changes, service restarts, or downloadable shell scripts, retrieve and follow:

Repository: `ry-arcana-blade/architecture`  
Branch: `main`  
Path: `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

The persistent machine/color registry currently includes:

- 🟦 `server0`
- 🟩 `ubuntu-cpu-llm`
- 🟧 `monitoring-kiosk`
- 🟪 `EVO-X2`

Do not duplicate or weaken those common operational safety rules in Crochet-specific work.

---

## 12. Active Task Selection

If a Crochet task is already active, recover and continue it instead of inventing a competing task.

If no active Crochet task exists and the user explicitly asks for new development:

1. read `docs/PROJECT_STATE.md`;
2. read the relevant `docs/ROADMAP.md` section;
3. inspect current `main`, open pull requests, CI, Gateway qualification evidence, and recent history;
4. identify one coherent unfinished Crochet objective;
5. include any necessary Crochet-specific MCP work within that objective when appropriate;
6. keep the work bounded and evidence-driven.

Do not use another Project's roadmap to fill a Crochet task-selection vacuum.

---

## 13. Cross-Project Dependencies

Cross-project inspection is allowed when needed to answer questions such as:

- Is a Gateway defect blocking Crochet qualification?
- Does the shared Gateway support the Crochet operation required by the current task?
- Is an inference endpoint available for Crochet evaluation?
- Does Architecture contain a newer common operating rule?
- Is a shared orchestration component preventing a Crochet MCP operation from completing?

When cross-project information is used:

- identify it as dependency/reference context;
- keep task ownership with Crochet when the objective remains Crochet-specific;
- do not continue unrelated work in the referenced Project;
- return to the Crochet objective after collecting the needed evidence.

---

## 14. Failure to Recover

If the user asks to recover or continue and no trustworthy active Crochet task can be established:

- do not guess;
- do not select Arcana Web work;
- do not select unrelated Arcana MCP work;
- do not choose a task merely because it is the newest visible Arcana task;
- report that no active Crochet task was recovered;
- identify the Crochet repository/project-state evidence checked, when useful.

If partial Crochet context exists, prefer the strongest durable repository evidence.

---

## 15. Scope Changes

The user may explicitly broaden Crochet work across repositories or projects.

Examples include:

- assigning a broader shared Gateway fix to the Crochet Project;
- requesting coordinated Crochet + Arcana MCP changes;
- asking Crochet to own a new MCP target or service;
- requesting a cross-project architecture change.

Such an explicit instruction may broaden scope for that task. Do not treat a temporary expansion as a permanent ownership change unless the user says so.

---

## 16. Governing Principle

Inside the Crochet Design Lab Project:

**Recover Crochet work as Crochet work. Crochet may own and implement the MCP work required to deliver its capabilities. Use other Arcana projects as dependencies when necessary, but do not automatically transfer a Crochet objective merely because MCP or Gateway code is involved.**
