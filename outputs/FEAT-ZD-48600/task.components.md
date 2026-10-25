# task.components: FEAT-ZD-48600

This file lists the components and layers that the Frontend lane creates or updates for FEAT-ZD-48600. Step: FE-03, "Identify the components/layers to be changed" (record:brief). The Frontend lane owns one task, TSK-07 (STORY-07, "Render the overdue risk table"), in repository ap_portal (feat.tasks, sections 1 and 3; task.repo_access, section 1).

This step only reads code. It changes no file in ap_portal. The current code is described from the ap_portal checkout on branch `feature/story` (tool:repo; task.repo_access, section 1; file:ap_portal/...). Statements marked **agent judgement** are this agent's proposal. No input states them as fact.

## 1. What the existing code does

| Item | Current state | Source |
|---|---|---|
| Repository layout | One source module, `src/ap_portal/index.js`, and one test file, `tests/portal.test.js`. The other files are `package.json`, `ci.mjs`, `README.md` and `CLAUDE.md`. There is no stylesheet, host page, router, service or transport layer | tool:repo; task.repo_access (section 2); file:ap_portal/package.json; file:ap_portal/ci.mjs; file:ap_portal/README.md; file:ap_portal/CLAUDE.md |
| Layers | One layer: pure view helpers that take plain values and return an HTML string | file:ap_portal/src/ap_portal/index.js ("AP portal view helpers") |
| `VERSION` | `'2.3.0'`. `package.json` has `"version": "2.3.0"` | file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json |
| `LABELS` | Constant map from aging buckets to labels | file:ap_portal/src/ap_portal/index.js |
| `formatAmount(value)` | `Number(value \|\| 0).toFixed(2)`. This formats through a binary float, against D-2026-044, so the new code must not use it. TQ-2 stays open | file:ap_portal/src/ap_portal/index.js; feat.trd (F-12); record:memory (D-2026-044) |
| `renderAgingSummary(buckets)` | Builds `<table class="aging-summary">` from template strings. It escapes nothing. The amount cell has `class="amount"` | file:ap_portal/src/ap_portal/index.js; feat.trd (F-13) |
| Exports | `module.exports = { VERSION, LABELS, formatAmount, renderAgingSummary }` | file:ap_portal/src/ap_portal/index.js |
| Callers | `formatAmount` is called only by `renderAgingSummary` and by the tests. The repository has no other source module | file:ap_portal/src/ap_portal/index.js; file:ap_portal/tests/portal.test.js; tool:repo |
| Existing tests | `formats amounts with two decimals` and `renders every bucket in order`. Both load the package with `require('ap_portal')` | file:ap_portal/tests/portal.test.js |
| CI | Runs `node --check` on every `src/ap_portal/*.js` file. Tests run with `node --test` and `NODE_PATH=src`. Line coverage of `src/**` must be at least 90. CI fails on any line in `src/ap_portal/*.js` that matches `console\.log` or `\beval\(`, or is longer than 140 characters | file:ap_portal/ci.mjs |

## 2. Components and layers to create or update

The smallest change that meets STORY-07 is additive and stays in one existing module. It adds one exported function, its private helpers and two constants to `src/ap_portal/index.js`. It adds no new module, no new layer and no dependency (feat.design B4.1, DD-1; feat.tasks TSK-07 "Files in budget").

| # | File / layer | Component | Kind | What changes | Why it is needed | Source |
|---|---|---|---|---|---|---|
| FC-1 | `src/ap_portal/index.js` (view helper layer) | `RISK_CLASSES = ['high', 'medium', 'low', 'unknown']` | New module constant | Allow-list of the four risk values | The row class comes only from known values. An unexpected value renders `risk-unknown` (AC-07.3, AC-07.8) | feat.design (B4.2, B4.3); feat.trd (IF-7.4); feat.stories (STORY-07) |
| FC-2 | `src/ap_portal/index.js` | `OVERDUE_HEADINGS = ['Invoice', 'Vendor', 'Days overdue', 'Amount']` | New module constant | The four headings, in order. The header row is built from this constant | AC-07.1, AC-07.2; NFR-10 ("Days overdue", never "Late") | feat.design (B4.2, B4.3); feat.trd (IF-7.2); feat.prd (AC-19, NFR-10) |
| FC-3 | `src/ap_portal/index.js` | `escapeHtml(value)` | New private helper (not exported) | `String(value)` with `&`, `<`, `>`, `"` and `'` escaped | `id` and `vendor` come from feed data, and the existing code escapes nothing (AC-07.7) | feat.design (B4.2, SE-2); feat.trd (IF-7.7, F-13, S-4) |
| FC-4 | `src/ap_portal/index.js` | `toTwoDecimalString(value)` | New private helper (not exported) | String-only. The value must be a string that matches `^-?\d+(\.\d{1,2})?$`. It is padded to two decimals as a string (`"980.5"` → `980.50`). Anything else throws `TypeError`. `Number()`, `parseFloat` and `formatAmount` are never used | NFR-3; D-2026-044; AC-07.5, AC-07.6 | feat.design (B4.2, B4.4, DD-5, DD-8); feat.tasks (TSK-07 implementation notes); record:memory (D-2026-044, L-011) |
| FC-5 | `src/ap_portal/index.js` | `riskClass(risk)` | New private helper (not exported) | `'risk-' + risk` when `risk` is in `RISK_CLASSES`, else `'risk-unknown'` | AC-07.3, AC-07.8 | feat.design (B4.2); feat.trd (IF-7.4) |
| FC-6 | `src/ap_portal/index.js` | `renderOverdueRisk(rows)` | New exported function | Returns `''` when `rows` is `undefined`, `null` or `[]`. Throws `TypeError` when `rows` is any other non-array value. Otherwise returns `<table class="overdue-risk">`, a plain header row `<tr><th>…</th></tr>`, one `<tr class="risk-…">` per row in the given order with cells id, vendor, days and amount (`class="amount"`), then `</table>`. `days` must pass `Number.isInteger`, else `TypeError`. No re-sorting, no Risk column or risk text, no row actions | STORY-07 AC-07.1 to AC-07.9; screen SC-1 | feat.design (HC-8, B4.2 algorithm, B4.4); feat.trd (IF-7.1 to IF-7.8); feat.tasks (TSK-07); feat.stories (STORY-07) |
| FC-7 | `src/ap_portal/index.js` | `module.exports` | Updated (additive) | Adds `renderOverdueRisk`: `{ VERSION, LABELS, formatAmount, renderAgingSummary, renderOverdueRisk }`. The helpers stay private | AC-07.10; every existing export is kept | feat.design (B4.2); feat.stories (AC-07.10); file:ap_portal/CLAUDE.md ("Keep existing public signatures; add new functions for new behaviour.") |
| FC-8 | `src/ap_portal/index.js` | `VERSION` | Updated | One minor step from `'2.3.0'`. The lane confirms the exact next number | DD-11; the version must move together with `package.json` | feat.tasks (section 2 "Version step", TSK-07 "Done when"); feat.design (DD-11) |
| FC-9 | `package.json` | `"version"` | Updated (version only) | Same value as FC-8 | Keeps `package.json` in step with `VERSION` | feat.tasks (TSK-07 "Files in budget", "Done when"); file:ap_portal/package.json |
| FC-10 | `tests/portal.test.js` (test layer) | New `node:test` cases | Updated (additive) | The B4.5 tests: `renders overdue risk table`, `maps every risk value to its class`, `returns empty string for no rows`, `shows amount strings unchanged`, `pads short amounts as strings`, `rejects non-string amounts`, `escapes id and vendor`, `unexpected risk renders risk-unknown`. Also the coverage cases listed in feat.tasks: non-array `rows` → `TypeError`; non-integer `days` → `TypeError`; `null` → `''`; an amount with an exponent, a thousands separator or three decimals → `TypeError`. The two existing tests stay unchanged | Line coverage of at least 90 in CI; T-12 to T-16 | feat.design (B4.5); feat.tasks (TSK-07 "Tests to add"); feat.trd (T-12 to T-16); file:ap_portal/ci.mjs |

FC-10 is listed because `tests/portal.test.js` is a file in the TSK-07 budget and this task changes it (feat.tasks TSK-07 "Files in budget").

### 2.1 Why this is the minimum

- **One module, no new file.** Agent judgement: a new `src/ap_portal/*.js` file would fall outside the TSK-07 file budget (feat.tasks, section 2 "Size budget" and TSK-07). Every CI check in `ci.mjs` would also apply to it (file:ap_portal/ci.mjs).
- **No change to existing code.** `LABELS`, `formatAmount`, `renderAgingSummary` and their tests stay as they are (feat.tasks TSK-07 "Must not change"; feat.design HC-9). `formatAmount` is not reused because it goes through a float (feat.trd F-12).
- **Same style as the existing renderer.** Like `renderAgingSummary`, `renderOverdueRisk` builds the table from template strings and `join('')`, with no `<thead>`/`<tbody>` (feat.design B4.2 step 4; feat.trd IF-7.2).
- **Helpers stay private.** Only `renderOverdueRisk` is added to the public exports (feat.design B4.2 `module.exports`).
- **Header from a constant.** Agent judgement: the header row is built by mapping over `OVERDUE_HEADINGS`, so each heading appears only once in the code.

## 3. Components and layers not changed

| Item | Reason | Source |
|---|---|---|
| `LABELS`, `formatAmount`, `renderAgingSummary` | Must not change. TQ-2 (the float inside `formatAmount`) is a separate decision | feat.tasks (TSK-07); feat.trd (C-9, F-12, TQ-2) |
| Existing tests in `tests/portal.test.js` | Must pass unchanged (T-16, AC-07.10) | feat.stories (AC-07.10); feat.trd (T-16) |
| `ci.mjs`, `CLAUDE.md`, `README.md` | Outside the TSK-07 file budget | feat.tasks (TSK-07 "Files in budget") |
| Host page, navigation, filters, row actions, transport for CT-1 | Out of scope for this design. A transport would re-trigger DES-01 | feat.design (A1, A3 "Transport for CT-1", A8, DD-3) |
| Stylesheet for the `risk-*` and `overdue-risk` classes | ap_portal has no stylesheet, and the design changes no host page. The component only sets the classes | tool:repo; feat.design (A1) |
| Other repositories (ap_aging serialiser, end-to-end harness) | Owned by the Backend lane (TSK-06) and the Testing Agent (TSK-08). The Frontend lane builds against the CT-1 field list only | feat.tasks (TSK-06, TSK-07 "Depends on", TSK-08); feat.design (A3 CT-1) |
| Database, Databricks | Not used | task.repo_access (section 1) |
| Secrets | None added | feat.design (SE-4) |

## 4. Input contract the component consumes (CT-1)

| Field | Type in CT-1 | How `renderOverdueRisk` uses it | Source |
|---|---|---|---|
| `id` | string | Escaped text in the Invoice cell | feat.design (A3 CT-1, B4.2) |
| `vendor` | string | Escaped text in the Vendor cell | feat.design (A3 CT-1, B4.2) |
| `days` | integer | Integer text in the Days overdue cell; `TypeError` if it fails `Number.isInteger` | feat.design (A3 CT-1, B4.4); feat.trd (IF-7.6) |
| `amount` | two-decimal string | Passed through `toTwoDecimalString` into the Amount cell, which has `class="amount"` | feat.design (A3 CT-1, B4.2); feat.trd (IF-7.3, IF-7.5) |
| `risk` | one of `high`, `medium`, `low`, `unknown` | Becomes the row class through `riskClass`; never shown as text | feat.design (A3 CT-1); feat.trd (IF-7.4, IF-7.8) |

Example (synthetic): `[{"id": "SYN-INV-004", "vendor": "Synthetic Vendor A", "days": 75, "amount": "1250.00", "risk": "high"}]` (feat.design A3).

## 5. CI constraints the change must respect

| Constraint | Effect on the components | Source |
|---|---|---|
| Line coverage of `src/**` at least 90 | Every branch of FC-3 to FC-6 needs a test, including each `TypeError` path | file:ap_portal/ci.mjs; feat.tasks (section 2 "CI bar") |
| No line in `src/ap_portal/*.js` matching `console\.log` or `\beval\(` | The rule applies to comments too. The new code has no logging | file:ap_portal/ci.mjs; feat.tasks (section 2 "CI bar"); feat.trd (IF-7.9) |
| No line longer than 140 characters | The table template is split across lines where needed | file:ap_portal/ci.mjs |
| `node --check` on each source file | CommonJS syntax for Node.js 22; the existing `'use strict'` line stays | file:ap_portal/ci.mjs; file:ap_portal/CLAUDE.md; file:ap_portal/src/ap_portal/index.js |

## 6. Traceability

| Component | Story AC | TRD | Design |
|---|---|---|---|
| FC-1, FC-5 | AC-07.3, AC-07.8 | IF-7.4; T-12, T-15 | B4.2; HC-8 |
| FC-2 | AC-07.1, AC-07.2 | IF-7.2; T-12 | B4.2 |
| FC-3 | AC-07.7 | IF-7.7; T-15 | B4.2; SE-2 |
| FC-4 | AC-07.5, AC-07.6 | IF-7.5; T-14 | B4.2; DD-5, DD-8 |
| FC-6 | AC-07.1 to AC-07.4, AC-07.9 | IF-7.1 to IF-7.8; T-12, T-13 | B4.2, B4.4; HC-8 |
| FC-7 | AC-07.10 | C-9; T-16 | B4.2 |
| FC-8, FC-9 | TK-07.5 | T-A6 | DD-11 |
| FC-10 | AC-07.1 to AC-07.10 | T-12 to T-16 | B4.5 |

Sources: feat.stories (STORY-07); feat.trd (IF-7, section 5); feat.design (B4); feat.tasks (TSK-07).

## 7. Open questions (none blocking)

| ID | Question | Interim, as in the approved design | Owner (proposed) |
|---|---|---|---|
| CQ-FE-1 | CT-1 types `id` and `vendor` as strings. No input says what to do when a row is missing either field or gives a non-string. The design's `escapeHtml` uses `String(value)`, so a missing value would render as the text `undefined`. Should `renderOverdueRisk` throw `TypeError` instead, as it does for `amount` and `days` (DD-8)? | Follow feat.design B4.2: `String(value)`, then escape | Architect |
| CQ-FE-2 | Who styles the `overdue-risk` table and the `risk-*` row classes? ap_portal has no stylesheet, and the host page is out of scope for this design | The component sets the classes only | UX lead |

Carried, unchanged: TQ-2, TQ-6, TQ-7, TQ-11 (feat.trd 7.2); OQ-B8, OQ-B9, OQ-B10 (feat.prd); RA-1 (branch name `feature/story` vs `feature/<story>`) and RA-2 (remote write access not yet tested) (task.repo_access, section 4). The pending markers on AC-07.2, AC-07.6, AC-07.7, AC-07.8 and AC-07.9 stay as in feat.tasks TSK-07.
