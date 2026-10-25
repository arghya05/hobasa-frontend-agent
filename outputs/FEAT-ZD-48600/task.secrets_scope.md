# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Frontend task of FEAT-ZD-48600, from step FE-02 Access to secrets. It lists secret names only and records no values.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-02 Access to secrets | record:brief |
| Frontend task | TSK-07 Render the overdue risk table (STORY-07), ap_portal | feat.tasks (section 1, TSK-07) |
| Repository | ap_portal | feat.tasks (TSK-07); tool:repo-access |
| Permission table | org.permission_table is restricted and passed by reference only, so this step did not read it | record:brief |

## 1. Secrets the task needs

**None.** TSK-07 adds a pure render function, `renderOverdueRisk(rows)`, together with constants and private helpers, in `src/ap_portal/index.js`. It also steps the version in `package.json` and adds tests in `tests/portal.test.js` (feat.tasks TSK-07). The feature has no transport layer (feat.tasks TSK-06, "No transport layer"), so the task makes no network call and uses no data store or external service.

Code check of the ap_portal checkout:

| Check | Result | Source |
|---|---|---|
| `src/ap_portal/index.js` | Pure view helpers (`formatAmount`, `renderAgingSummary`) and constants. It has no `require`, no `process.env`, no network call and no URL | file:ap_portal/src/ap_portal/index.js |
| `package.json` | No dependencies are declared. The only script is `ci` | file:ap_portal/package.json |
| `ci.mjs` (CI) | Copies the process environment and sets only `NODE_PATH: 'src'`. It reads no secret | file:ap_portal/ci.mjs |
| `tests/portal.test.js` | Requires only `node:test`, `node:assert` and `ap_portal`. It reads no secret and calls no service | file:ap_portal/tests/portal.test.js |

## 2. Secrets granted for the task's environments

| Environment | Secret names granted | Source |
|---|---|---|
| Local build and unit tests (`node ci.mjs`) | None | tool:repo-access |
| CI pipeline | None | tool:repo-access |
| dev (integration) / main (released) | None | tool:repo-access |

The tool registry for ap_portal states that the permission table requires and grants no secrets for this repository (tool:repo-access).

## 3. Gap and access requests

| Required | Granted | Gap | Request raised |
|---|---|---|---|
| None | None | None | None needed |

What the task needs matches what is granted. No access is excess, so nothing needs flagging, and no access is missing, so nothing needs requesting.

## 4. Re-entry rule

If a later change adds a transport layer, an API call or a package dependency to ap_portal, the secrets scope must be checked again. A transport layer would also re-trigger DES-01 (feat.tasks TSK-06). In that case FE-02 runs again before FE-04 Coding.

## 5. Open questions

None.


[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)