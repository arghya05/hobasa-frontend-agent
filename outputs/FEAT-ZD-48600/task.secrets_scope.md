# task.secrets_scope: FEAT-ZD-48600 (Frontend, ap_portal)

FE-02 Access to secrets. This file lists the secrets the Frontend task needs and the secrets granted for it. It gives names only and holds no secret values.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-02 Access to secrets | record:brief |
| Task | TSK-07, STORY-07: render the overdue risk table SC-1 (`renderOverdueRisk`) | feat.tasks (sections 1, 3 TSK-07) |
| Repository | ap_portal | feat.tasks (TSK-07) |
| Inputs | feat.tasks; org.permission_table (restricted, passed by reference only) | record:brief |

## 1. Secrets needed by the task

**None.** TSK-07 needs no secret in any environment.

| Check | Finding | Source |
|---|---|---|
| Scope of the change | A pure render helper in `src/ap_portal/index.js`, plus `package.json` (version only) and `tests/portal.test.js` | feat.tasks (TSK-07 "Files in budget") |
| Data the function takes | The caller passes in rows that already have the CT-1 shape. TSK-07 depends only on the CT-1 field list, not on TSK-06 code | feat.tasks (TSK-07 "Depends on") |
| Transport, API or service call | None. The design adds no transport layer, because a transport would re-trigger DES-01 | feat.tasks (TSK-06 "Must not change") |
| New technology or dependency | None; Node.js 22 CommonJS only | feat.tasks (intro, DD-1) |
| Package dependencies | `package.json` declares no dependencies, so no registry token is needed | file:ap_portal/package.json |
| Secret use in today's code | `src/ap_portal/index.js` reads no environment variable and calls no external service | file:ap_portal/src/ap_portal/index.js |
| Secret use in CI | `ci.mjs` copies the process environment and sets only `NODE_PATH: 'src'`. It reads no secret | file:ap_portal/ci.mjs |
| Secret use in tests | `tests/portal.test.js` loads only `node:test`, `node:assert` and `ap_portal`. Its tests call `formatAmount` and `renderAgingSummary` on literal values and read no environment variable | file:ap_portal/tests/portal.test.js |

## 2. Secrets granted (names only)

| Environment | Secrets granted | Source |
|---|---|---|
| Local / unit tests (`node ci.mjs`) | None | tool:repo-access |
| dev (integration) | None | tool:repo-access |
| main (released) | None | tool:repo-access |

The tool registry says the permission table requires and grants no secrets for ap_portal. What the task needs matches what is granted, so no access request is raised.

## 3. Access requests

None. The task needs no secret that is not already granted.

## 4. Re-entry condition

This scope holds while TSK-07 stays within its file budget. FE-02 runs again if a later change adds a transport, an external API call or a package dependency that needs a token. Any request for access then goes to the owner of the permission table (agent judgement).

## 5. Open questions

None, blocking or non-blocking.

## 6. Validation check

| Check | Result | Source |
|---|---|---|
| Names only; no secret values in this file | Pass | record:brief |
| Required secrets covered by grants | Pass (none required, none granted) | tool:repo-access; feat.tasks (TSK-07) |
| Conflict with an approved output | None | feat.tasks |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)
