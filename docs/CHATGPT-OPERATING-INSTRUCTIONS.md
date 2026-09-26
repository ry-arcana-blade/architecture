# ChatGPT Operating Instructions

**Status:** AUTHORITATIVE  
**Repository:** `ry-arcana-blade/architecture`  
**Branch:** `main`  
**Path:** `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

## Purpose

These instructions govern ChatGPT-assisted shell, deployment, diagnostic, test, security, restart, administrative, infrastructure, and related technical work across Arcana Blade, Arcana MCP / Secure Agent Gateway, Crochet Design Lab, Architecture, operations dashboard work, kiosk administration, and related current or future projects.

When this document is available, treat it as the authoritative source of truth for the operating rules described here. Do not silently substitute remembered, stale, or inferred procedures for the current contents of this file.

If a user instruction in the current conversation explicitly overrides a rule in this document, follow the user's explicit instruction for that task unless doing so would violate a higher-priority safety or platform requirement.

---

## 1. Persistent Machine Identity Registry

Always make the target machine unmistakable.

Persistent machine/color mappings:

- 🟦 `server0`
- 🟩 `ubuntu-cpu-llm`
- 🟧 `monitoring-kiosk`
- 🟪 `EVO-X2`

Never swap these mappings.

Assign a new persistent color to every new server or managed host. Once assigned, keep that mapping stable in later work.

Use the matching color consistently in headings, run-target labels, banners, and execution instructions.

---

## 2. Command Delivery

Prefer MCP/connectors and fixed read-only automation when practical.

Any delivered file containing shell commands must be a downloadable `.sh` file, never a `.txt` file.

The normal workflow is:

1. Open the `.sh`.
2. Select All / Copy.
3. Paste the entire contents directly into an existing interactive SSH Bash prompt.

Design every substantial `.sh` for that workflow unless the user explicitly requests otherwise.

Before each file link, show the target and classification, for example:

- 🟦 RUN ON: `server0` — READ-ONLY
- 🟩 RUN ON: `ubuntu-cpu-llm` — MODIFIES CONFIGURATION
- 🟧 RUN ON: `monitoring-kiosk` — RESTARTS SERVICE
- 🟪 RUN ON: `EVO-X2` — DEPLOYS CODE

Never make the user infer which machine should receive a command.

---

## 3. Copy/Paste Safety

For substantial shell work:

1. Print the START banner at top level.
2. Put all operational logic inside a parenthesized subshell: `(...)`.
3. Keep strict mode, variables, functions, traps, `cd`, hostname guards, sudo handling, loops, conditions, pipelines, and every `exit` inside that subshell.
4. Use an `EXIT` trap inside the subshell to print final status and the END banner.

At the parent interactive-shell level, never use:

- `exit`
- `logout`
- `return`
- `exec`
- traps
- `cd`
- persistent variable definitions
- persistent function definitions
- `set -e`
- `set -u`
- `set -o pipefail`
- `set -euo pipefail`

A failure must terminate only the isolated subshell, never the user's SSH session.

Never append a top-level `exit $?` after the closing `)`.

Do not call a script copy/paste-safe merely because it parses successfully.

---

## 4. Required Validation Before Delivering a Shell Script

Before delivering any substantial `.sh`:

1. Run `bash -n` against the **actual file written to disk**, not only an in-memory source string.
2. Read that generated file back and verify it is byte-for-byte the intended content.
3. Check for unmatched quotes.
4. Check for malformed heredocs.
5. Check for broken line continuations.
6. Check for incomplete shell constructs.
7. Confirm dangerous shell state exists only inside the isolated subshell.
8. Simulate pasting the **exact full generated file read back from disk** into an interactive Bash parent shell.
9. Deliberately exercise at least one failure path.
10. Verify the END banner prints on failure.
11. Run another command in the same simulated parent shell after the script ends.
12. Verify that command succeeds, proving the parent interactive shell remains usable.
13. Verify the complete paste returns Bash to its primary prompt rather than leaving it at a continuation prompt such as `>`.

Parsing alone is not sufficient evidence of paste safety.

### 4.1 Paste-safe launcher pattern

For nontrivial diagnostics, deployment transactions, or administrative logic, prefer a **small copy/paste launcher plus repository-owned reviewed implementation** rather than embedding the full implementation in the downloadable shell file.

Use this pattern when the operation would otherwise require large embedded Python, nested heredocs, long here-doc bodies, large quoted payloads, generated source code, or other complex multiline shell parsing:

1. Put the complex implementation in the appropriate reviewed Git repository.
2. Add focused tests for that implementation.
3. Require the implementation's exact repository head to pass its normal CI gates before host execution.
4. Make the downloadable `.sh` only a compact launcher that:
   - prints the normal START banner;
   - enters the isolated subshell;
   - validates hostname/user and required tools;
   - verifies the exact reviewed branch/head;
   - materializes only the reviewed implementation;
   - verifies its exact Git blob/hash;
   - syntax-checks or otherwise validates the materialized implementation when applicable;
   - executes it with the fixed reviewed runtime/environment;
   - prints final status and the END banner.
5. Do not embed a second large program inside the copy/paste launcher merely to avoid adding a reviewed repository file.
6. Avoid heredocs and embedded language payloads in the downloadable launcher when the same logic can live in the reviewed repository.

The launcher should be intentionally small enough to be reliable when the user opens the file, Select All / Copy, and pastes it into the existing SSH Bash prompt.

### 4.2 Primary-prompt prerequisite

Before asking the user to paste a shell launcher, require that the terminal is at the normal Bash **primary prompt** (for example, a prompt ending in `# ChatGPT Operating Instructions

**Status:** AUTHORITATIVE  
**Repository:** `ry-arcana-blade/architecture`  
**Branch:** `main`  
**Path:** `docs/CHATGPT-OPERATING-INSTRUCTIONS.md`

## Purpose

These instructions govern ChatGPT-assisted shell, deployment, diagnostic, test, security, restart, administrative, infrastructure, and related technical work across Arcana Blade, Arcana MCP / Secure Agent Gateway, Crochet Design Lab, Architecture, operations dashboard work, kiosk administration, and related current or future projects.

When this document is available, treat it as the authoritative source of truth for the operating rules described here. Do not silently substitute remembered, stale, or inferred procedures for the current contents of this file.

If a user instruction in the current conversation explicitly overrides a rule in this document, follow the user's explicit instruction for that task unless doing so would violate a higher-priority safety or platform requirement.

---

## 1. Persistent Machine Identity Registry

Always make the target machine unmistakable.

Persistent machine/color mappings:

- 🟦 `server0`
- 🟩 `ubuntu-cpu-llm`
- 🟧 `monitoring-kiosk`
- 🟪 `EVO-X2`

Never swap these mappings.

Assign a new persistent color to every new server or managed host. Once assigned, keep that mapping stable in later work.

Use the matching color consistently in headings, run-target labels, banners, and execution instructions.

---

## 2. Command Delivery

Prefer MCP/connectors and fixed read-only automation when practical.

Any delivered file containing shell commands must be a downloadable `.sh` file, never a `.txt` file.

The normal workflow is:

1. Open the `.sh`.
2. Select All / Copy.
3. Paste the entire contents directly into an existing interactive SSH Bash prompt.

Design every substantial `.sh` for that workflow unless the user explicitly requests otherwise.

Before each file link, show the target and classification, for example:

- 🟦 RUN ON: `server0` — READ-ONLY
- 🟩 RUN ON: `ubuntu-cpu-llm` — MODIFIES CONFIGURATION
- 🟧 RUN ON: `monitoring-kiosk` — RESTARTS SERVICE
- 🟪 RUN ON: `EVO-X2` — DEPLOYS CODE

Never make the user infer which machine should receive a command.

---

## 3. Copy/Paste Safety

For substantial shell work:

1. Print the START banner at top level.
2. Put all operational logic inside a parenthesized subshell: `(...)`.
3. Keep strict mode, variables, functions, traps, `cd`, hostname guards, sudo handling, loops, conditions, pipelines, and every `exit` inside that subshell.
4. Use an `EXIT` trap inside the subshell to print final status and the END banner.

At the parent interactive-shell level, never use:

- `exit`
- `logout`
- `return`
- `exec`
- traps
- `cd`
- persistent variable definitions
- persistent function definitions
- `set -e`
- `set -u`
- `set -o pipefail`
- `set -euo pipefail`

A failure must terminate only the isolated subshell, never the user's SSH session.

Never append a top-level `exit $?` after the closing `)`.

Do not call a script copy/paste-safe merely because it parses successfully.

---

), not the secondary continuation prompt `>`.

If a previous paste left Bash at `>`:

1. Do not paste a new launcher into that continuation context.
2. Press `Ctrl+C` once to cancel the incomplete construct.
3. Confirm the normal primary prompt has returned.
4. Only then paste the next complete launcher.

A new script pasted at a `>` prompt is parsed as part of the unfinished previous construct and must not be treated as having executed, even if its text appears in the terminal transcript.

---

## 5. Hostname Guards

Important diagnostic, deployment, configuration, security, restart, and administrative scripts must validate the hostname before meaningful work.

The hostname guard belongs inside the isolated subshell.

The START banner must print before hostname validation.

When a task targets a known host, compare the actual hostname against the expected hostname and fail closed inside the subshell if it does not match.

When user identity is materially important, validate the expected user as well.

---

## 6. Standard Banners

### 🟦 server0

```text
🟦 ╔══════════════════════════════════════════════════════╗
🟦 ║ COPY/PASTE START — server0                           ║
🟦 ╚══════════════════════════════════════════════════════╝
...
🟦 ╔══════════════════════════════════════════════════════╗
🟦 ║ COPY/PASTE END — server0                             ║
🟦 ╚══════════════════════════════════════════════════════╝
```

### 🟩 ubuntu-cpu-llm

```text
🟩 ╔══════════════════════════════════════════════════════╗
🟩 ║ COPY/PASTE START — ubuntu-cpu-llm                    ║
🟩 ╚══════════════════════════════════════════════════════╝
...
🟩 ╔══════════════════════════════════════════════════════╗
🟩 ║ COPY/PASTE END — ubuntu-cpu-llm                      ║
🟩 ╚══════════════════════════════════════════════════════╝
```

### 🟧 monitoring-kiosk

```text
🟧 ╔══════════════════════════════════════════════════════╗
🟧 ║ COPY/PASTE START — monitoring-kiosk                  ║
🟧 ╚══════════════════════════════════════════════════════╝
...
🟧 ╔══════════════════════════════════════════════════════╗
🟧 ║ COPY/PASTE END — monitoring-kiosk                    ║
🟧 ╚══════════════════════════════════════════════════════╝
```

### 🟪 EVO-X2

```text
🟪 ╔══════════════════════════════════════════════════════╗
🟪 ║ COPY/PASTE START — EVO-X2                            ║
🟪 ╚══════════════════════════════════════════════════════╝
...
🟪 ╔══════════════════════════════════════════════════════╗
🟪 ║ COPY/PASTE END — EVO-X2                              ║
🟪 ╚══════════════════════════════════════════════════════╝
```

The END banner must print even when the subshell fails. Use an `EXIT` trap inside the subshell to guarantee this behavior.

---

## 7. Classification

Classify each delivered shell file accurately.

Use classifications such as:

- READ-ONLY
- MAKES CHANGES
- MODIFIES CONFIGURATION
- DEPLOYS CODE
- STARTS SERVICE
- STOPS SERVICE
- RESTARTS SERVICE
- RESTARTS SERVICES
- INSTALLS SOFTWARE
- REMOVES SOFTWARE
- CHANGES NETWORKING
- CHANGES SECURITY CONFIGURATION

Do not hide mutations inside READ-ONLY work.

If a script both inspects state and mutates state, classify it according to the mutation.

---

## 8. Bounded Change Safety

For important changes:

1. Inspect current state.
2. Guard assumptions.
3. Make one bounded mutation.
4. Collect evidence.
5. Analyze the evidence before the next mutation.

Prefer small, reversible steps over broad multi-system changes.

Useful guards include, where relevant:

- hostname
- current user
- Git branch
- Git HEAD
- clean/dirty working-tree state
- expected remote URL
- config hashes
- service PID
- systemd InvocationID
- service restart count
- listeners and bound ports
- file presence
- file ownership and mode
- container identity
- image digest
- service identity
- expected process command line
- expected repository revision

Create rollback evidence before significant configuration changes.

Do not proceed across a failed guard merely because the intended mutation appears harmless.

---

## 9. Sudo

If elevation is needed:

- Run `sudo -v` inside the isolated subshell.
- Allow the normal terminal password prompt.
- Never capture the sudo password.
- Never echo the sudo password.
- Never store the sudo password in a variable, file, command argument, environment variable, or generated artifact.

Do not use password-handling workarounds to make an unattended script out of an interactive sudo workflow unless the user explicitly requests a different secure automation design.

---

## 10. Read-Only Work

A READ-ONLY script must not:

- write files
- edit configuration
- install or remove packages
- restart, start, stop, enable, or disable services
- send process signals
- mutate Git state
- change permissions or ownership
- change network state
- change container state
- change system settings
- consume or delete state records
- rotate or truncate logs

Commands that update metadata, caches, timestamps, package indexes, Git refs, or other persistent state are not read-only merely because they do not alter application data.

---

## 11. Git and Deployment Work

For important Git-backed deployments or configuration changes, guard the relevant repository state before mutation.

Where applicable, verify:

- repository path
- repository remote
- expected branch
- exact HEAD or reviewed commit
- clean working tree
- expected tags
- deployment mode
- required secrets/config files exist
- ignored local environment files remain preserved
- target service/container identity matches the intended application

When deploying an exact reviewed revision, do not silently advance to newer unrelated commits.

Create rollback evidence before significant deployment mutations.

---

## 12. Service and Restart Work

Before restarting or replacing an important service, collect enough pre-change evidence to identify what was running.

Where useful, capture:

- service active state
- PID
- InvocationID
- restart count
- executable or container identity
- listeners
- relevant recent journal evidence
- current revision/config hash

After the bounded mutation, collect the corresponding post-change evidence and compare it before proceeding.

Do not describe a restart as successful merely because the restart command returned zero.

---

## 13. Output Return

When output is needed for analysis, ask the user to paste everything from:

`COPY/PASTE START`

through the matching:

`COPY/PASTE END`

inclusive.

Design diagnostic output so that returned evidence is sufficiently complete to analyze without requiring the user to manually extract scattered lines.

---

## 14. MCP and Connector Preference

Prefer MCP/connectors and fixed read-only automation when those mechanisms can safely retrieve authoritative state without asking the user to run shell commands.

Use shell commands when host-local state, deployment actions, operating-system evidence, or other machine-local behavior cannot be safely or adequately obtained through the available connectors.

Do not use a shell mutation when an available connector can perform the same task more safely and with clearer scope.

---

## 15. Failure Behavior

Fail closed when an important assumption is not satisfied.

A failure inside a delivered shell script must:

- remain inside the isolated subshell
- preserve the parent SSH session
- emit useful failure evidence
- print final status
- print the matching END banner

Do not suppress a failed hostname, identity, revision, configuration, or service guard in order to continue the requested mutation.

---

## 16. Scope

Apply these instructions across:

- Arcana Blade
- Arcana MCP / Secure Agent Gateway
- Crochet Design Lab
- Architecture
- operations dashboard work
- kiosk administration
- infrastructure and deployment work
- related future projects
- future managed hosts unless explicitly excluded

These rules remain in force until the user explicitly changes them or this authoritative document is updated.
