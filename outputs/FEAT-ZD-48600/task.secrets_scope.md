# task.secrets_scope: FEAT-ZD-48600 (Frontend lane)

FE-02 Access to secrets. This file records the secrets the Frontend lane needs for its task, and the secrets granted for them. It lists names only and holds no secret values.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-02 Access to secrets | record:brief |
| Frontend task | TSK-07 (STORY-07): render the overdue risk table SC-1 | feat.tasks (section 1, TSK-07) |
| Repository | ap_portal | feat.tasks (section 1, TSK-07) |
| Permission source | org.permission_table (restricted: passed by reference only, summarised by the tool view) | record:brief; tool:repo-access |

## 1. Secrets the task needs

**None.** The evidence:

| Check | Finding | Source |
|---|---|---|
| What the task builds | `renderOverdueRisk(rows)` is an exported function that returns an HTML string from the rows it is given. Its private helpers are `escapeHtml`, `toTwoDecimalString` and `riskClass`, and it uses the constants `RISK_CLASSES` and `OVERDUE_HEADINGS`. Invalid input raises `TypeError`. | feat.tasks (TSK-07 goal and implementation notes) |
| Files in the task's budget | `src/ap_portal/index.js`, `package.json` (version only) and `tests/portal.test.js` | feat.tasks (TSK-07 "Files in budget") |
| Calls to other services | None. The task depends only on the CT-1 field list. It can be built alongside TSK-06 and does not call TSK-06's code. There is no transport layer: TSK-06 rules one out, because adding one would re-trigger DES-01. | feat.tasks (TSK-06 "Must not change"; TSK-07 "Depends on") |
| Current source | `index.js` exports `VERSION`, `LABELS`, `formatAmount` and `renderAgingSummary`. It has no `require`, no network calls and does not read the environment. | file:ap_portal/src/ap_portal/index.js |
| Dependencies | `package.json` declares no dependencies and has one script, `ci` (`node ci.mjs`). No registry token is needed. | file:ap_portal/package.json |
| CI | `ci.mjs` copies `process.env` and adds `NODE_PATH: 'src'`. It reads no named secret. Its only steps are syntax checks, `node --test` with coverage, and static line checks. | file:ap_portal/ci.mjs |
| Tests | `tests/portal.test.js` loads only `node:test`, `node:assert` and `ap_portal`. Its tests call `formatAmount` and `renderAgingSummary` with literal values only, with no network access, file reads or environment reads. | file:ap_portal/tests/portal.test.js |
| Test data | The end-to-end proof (TSK-08) uses a synthetic feed only (`SYN-INV-*`). The Testing Agent owns it, not this lane. | feat.tasks (TSK-08 implementation notes) |

## 2. Secrets granted

| Environment | Secret names granted | Source |
|---|---|---|
| ap_portal, every environment the task uses (local build, CI) | None | tool:repo-access ("no secrets are required or granted for this repository (permission table)") |

## 3. Gap between need and grant

| Needed | Granted | Gap | Access request |
|---|---|---|---|
| None | None | None | No request raised. Asking for a secret the task does not need would grant more access than needed. |

## 4. Outcome

The Frontend lane can start FE-03 and FE-04 for TSK-07 without any secret. This file grants no secret, and none should be added to the ap_portal branch, CI settings or test data for this task. If a later step adds a network call, a dependency that needs a registry token, or a deployed environment, FE-02 must run again, because that would be a re-entry trigger.

## 5. Open questions

None blocking. None non-blocking.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)
