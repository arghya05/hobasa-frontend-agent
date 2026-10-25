# task.components: FEAT-ZD-48600

Components and layers the Frontend lane will create or update for FEAT-ZD-48600 (ZD-48600, "Show which overdue invoices belong to vendors that usually pay late"). This is FE-03, attempt 1. The lane owns one task: TSK-07 / STORY-07, "Render the overdue risk table", in the ap_portal repository (feat.tasks, section 1; task.repo_access).

This step reads the existing code and lists the smallest set of changes that meets STORY-07. It changes no code; FE-04 writes the code. Statements marked **agent judgement** are this agent's proposals. No input states them as fact.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-03 Identify the components/layers to be changed (attempt 1) | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), feat.stories (0590ea4), feat.prd (908b58d), feat.trd (3637e51), feat.design (722a98f), task.repo_access (0097a2b) | record:brief |
| Task / story / HLD component | TSK-07 / STORY-07 / HC-8 (`renderOverdueRisk`) | feat.tasks (sections 1, 4); feat.design (A2) |
| Repository / branch | ap_portal, `feature/story` at `ff761ee` | task.repo_access |
| Code read | `src/ap_portal/index.js`, `tests/portal.test.js`, `package.json`, `ci.mjs`, `README.md`, `CLAUDE.md` | tool:repo; mcp:codegraph; file:ap_portal/ci.mjs; file:ap_portal/package.json; file:ap_portal/README.md; file:ap_portal/CLAUDE.md |

## 1. What the existing code does

ap_portal has **one layer**: a stateless view-helper module that builds HTML strings. It has no routing, no state, no API client, no stylesheet and no build step. The checkout contains `CLAUDE.md`, `README.md`, `ci.mjs`, `package.json`, `src/ap_portal/index.js` and `tests/portal.test.js` (tool:repo; file:ap_portal/package.json).

| Symbol in `src/ap_portal/index.js` | Line | What it does | Depended on by | Source |
|---|---|---|---|---|
| Header comment | 2 | "AP portal view helpers: render the aging summary returned by the ap_aging service as HTML." | — | mcp:codegraph |
| `VERSION` | 3 | `'2.3.0'` (also `"version": "2.3.0"` in `package.json`) | Exported | mcp:codegraph; file:ap_portal/package.json |
| `LABELS` | 4 | Bucket key → display label | `renderAgingSummary`; exported | mcp:codegraph |
| `formatAmount(value)` | 6 | `Number(value \|\| 0).toFixed(2)`. This goes through a binary float, which conflicts with D-2026-044 | `renderAgingSummary`; exported; existing test | mcp:codegraph; feat.trd (F-12); record:memory |
| `renderAgingSummary(buckets)` | 10 | `<table class="aging-summary">` from template strings. It escapes nothing; its labels are constants | Exported; existing test | mcp:codegraph; feat.trd (F-13) |
| `module.exports` | 15 | `{ VERSION, LABELS, formatAmount, renderAgingSummary }` | Tests via `require('ap_portal')` | mcp:codegraph; tool:repo |

**Tests.** `tests/portal.test.js` uses `node:test` and `node:assert` and loads the package with `require('ap_portal')`. It has two tests: `formats amounts with two decimals` and `renders every bucket in order` (tool:repo).

**Note on codegraph.** codegraph reports "no covering tests found" for `formatAmount` and `LABELS` (mcp:codegraph). Agent judgement: this is a false negative. Both are exercised by the existing tests, which load the module through `require('ap_portal')` with `NODE_PATH: 'src'`. The index most likely does not follow that resolution (file:ap_portal/ci.mjs; tool:repo).

**CI binding.** `ci.mjs` runs `node --check` on every `.js` file in `src/ap_portal`, then `node --test` with coverage of `src/**` and `NODE_PATH: 'src'`. It requires line coverage `>= 90`, at least one test run, no failures, no line matching `console.log` or `eval(`, and no line longer than 140 characters (file:ap_portal/ci.mjs).

## 2. Components to create or update (minimum change)

| # | File / layer | Component | Kind | Change | Why it is needed | Source |
|---|---|---|---|---|---|---|
| C-FE-1 | `src/ap_portal/index.js` (view helper) | `RISK_CLASSES` | New constant | `['high', 'medium', 'low', 'unknown']` | Allow-list for the row class (AC-07.3, AC-07.8) | feat.design (B4.2, B4.3); feat.trd (IF-7.4) |
| C-FE-2 | same | `OVERDUE_HEADINGS` | New constant | `['Invoice', 'Vendor', 'Days overdue', 'Amount']` | Header row in order (AC-07.1, AC-07.2). Uses "Days overdue", never "Late" | feat.design (B4.2); feat.prd (NFR-10) |
| C-FE-3 | same | `escapeHtml(value)` | New private helper | `String(value).replace(/[&<>"']/g, …)` with one lookup map | Escapes `id` and `vendor` (AC-07.7) | feat.design (B4.2, SE-2); feat.trd (IF-7.7) |
| C-FE-4 | same | `toTwoDecimalString(value)` | New private helper | String-only. Checks `typeof value === 'string'` and the pattern `^-?\d+(\.\d{1,2})?$`, pads the fraction with zeros to two digits, and throws `TypeError` otherwise | Shows the amount without `Number()`, `parseFloat` or `formatAmount` (AC-07.5, AC-07.6) | feat.design (B4.2, B4.4, DD-5, DD-8); feat.tasks (TSK-07 notes) |
| C-FE-5 | same | `riskClass(risk)` | New private helper | `RISK_CLASSES.includes(risk) ? 'risk-' + risk : 'risk-unknown'` | Feed data can never inject a class name (AC-07.3, AC-07.8) | feat.design (B4.2); feat.trd (IF-7.4) |
| C-FE-6 | same | `renderOverdueRisk(rows)` | New exported function | Returns `''` for `undefined`, `null` or `[]`. Throws `TypeError` for any other non-array value. Otherwise returns `<table class="overdue-risk">`, a plain header row, then one `<tr class="risk-…">` per row in the given order (id, vendor, days, `<td class="amount">` amount), then `</table>`. Throws `TypeError` when `days` fails `Number.isInteger`. No re-sorting, no Risk column or risk text, no row actions | HC-8; AC-07.1 to AC-07.4, AC-07.9 | feat.design (B4.2 steps 1–4, B4.4); feat.tasks (TSK-07); feat.stories (AC-07.1 to AC-07.9) |
| C-FE-7 | same | `module.exports` (line 15) | Updated | Add `renderOverdueRisk` and leave the existing four names unchanged. The helpers stay private | AC-07.10 | feat.design (B4.2); feat.stories (AC-07.10); mcp:codegraph |
| C-FE-8 | same | `VERSION` (line 3) | Updated | Next minor version. The lane confirms the number | DD-11; TK-07.5 | feat.design (DD-11); feat.tasks (section 2) |
| C-FE-9 | `package.json` | `"version"` | Updated | Same value as `VERSION`. Nothing else in the file changes | Keeps `VERSION` and `"version"` in step | feat.tasks (TSK-07 "Done when"); file:ap_portal/package.json |
| C-FE-10 | `tests/portal.test.js` (test layer) | New `node:test` cases | Added after the existing tests | The B4.5 tests plus the CL-FE6 coverage cases (see section 4) | TK-07.3; CI coverage of at least 90% | feat.design (B4.5); feat.tasks (TSK-07 "Tests to add") |

### How this keeps the change small (agent judgement)

1. **No new file.** All of the code goes into `src/ap_portal/index.js`. A new module would add a second `require` path, and `ci.mjs` would also have to compile it, so nothing is gained (feat.design DD-2; task.repo_access section 3; file:ap_portal/ci.mjs).
2. **Append, don't rewrite.** Put the two constants and the four functions between `renderAgingSummary` (ends at line 13) and `module.exports` (line 15). Lines 4–13 stay byte-for-byte the same (mcp:codegraph; feat.tasks TSK-07 "Must not change").
3. **Two existing lines change**: `VERSION` (line 3) and `module.exports` (line 15). The header comment on line 2 may be widened to mention the overdue risk table. That is optional; it is a comment only (mcp:codegraph).
4. **Build the header from `OVERDUE_HEADINGS`** with `map` and `join('')`. Follow the `renderAgingSummary` template-string style; no DOM or templating library (feat.design B4.2 step 4; DD-1).
5. **Keep the helpers small and private.** `riskClass` and `escapeHtml` are single expressions. The design names them as helpers, so they stay as functions rather than being inlined. This keeps the code traceable to B4.2 (feat.design B4.2).
6. **Don't reuse `formatAmount`**, even though it already gives two decimals. Reusing it would break D-2026-044 and fail T-14 (feat.trd F-12, R-2; record:memory).

### Unchanged (must not change)

| Item | Reason | Source |
|---|---|---|
| `LABELS`, `formatAmount`, `renderAgingSummary` and their two tests | Signature rule; AC-07.10; T-16 | feat.tasks (TSK-07); feat.design (HC-9); file:ap_portal/CLAUDE.md |
| `ci.mjs` | The CI script is not in TSK-07's file budget | feat.tasks (TSK-07 "Files in budget") |
| `README.md`, `CLAUDE.md` | Not in the file budget | feat.tasks (TSK-07 "Files in budget") |
| Other repositories (ap_aging and the rest) | TSK-07 depends only on the CT-1 field list | feat.tasks (TSK-07 "Depends on"); task.repo_access (section 4) |

## 3. Interface consumed: CT-1

`rows` is the result of `JSON.parse` on the ap_aging payload. It is an array of `{id: string, vendor: string, days: integer, amount: string, risk: string}`, already in the right order. `amount` is a two-decimal string with no exponent and no thousands separator. `risk` is one of `high`, `medium`, `low` or `unknown` (feat.design A3 CT-1, B4.3). This repository adds no transport (feat.design DD-3).

## 4. Branches the tests must cover (for the 90% line-coverage bar)

| Branch in `renderOverdueRisk` / helpers | Test | Source |
|---|---|---|
| `undefined`, `null`, `[]` → `''` | `returns empty string for no rows`, plus `null` | feat.design (B4.5); feat.tasks (CL-FE6 list) |
| Non-array `rows` → `TypeError` | CL-FE6 case | feat.tasks (TSK-07) |
| Each of the four risk classes; an unexpected risk → `risk-unknown` | `maps every risk value to its class`, `unexpected risk renders risk-unknown` | feat.design (B4.5) |
| Amount unchanged; padded from `"980.5"`; non-string → `TypeError`; exponent, thousands separator or three decimals → `TypeError` | `shows amount strings unchanged`, `pads short amounts as strings`, `rejects non-string amounts`, CL-FE6 cases | feat.design (B4.5); feat.tasks (TSK-07) |
| Non-integer `days` → `TypeError` | CL-FE6 case | feat.tasks (TSK-07) |
| Escaping of `<`, `&`, `"` in `id` and `vendor` | `escapes id and vendor` | feat.design (B4.5) |
| Full table shape and row order | `renders overdue risk table` | feat.design (B4.5); feat.stories (AC-07.1) |

## 5. Traceability

| Component | Story AC | TRD tests |
|---|---|---|
| C-FE-1, C-FE-5 | AC-07.3, AC-07.8 | T-12, T-15 |
| C-FE-2, C-FE-6 | AC-07.1, AC-07.2, AC-07.4, AC-07.9 | T-12, T-13 |
| C-FE-3 | AC-07.7 | T-15 |
| C-FE-4 | AC-07.5, AC-07.6 | T-14 |
| C-FE-7, unchanged items | AC-07.10 | T-16 |
| C-FE-8, C-FE-9 | TK-07.5 | — |

Sources: feat.stories (STORY-07); feat.trd (T-12 to T-16); feat.design (B4.5).

## 6. Assumptions (agent judgement)

| ID | Assumption |
|---|---|
| CA-1 | The helpers are not exported, as the `module.exports` list in feat.design B4.2 shows. Tests reach them through `renderOverdueRisk` |
| CA-2 | The new code goes between `renderAgingSummary` and `module.exports`, so the diff touches no existing logic |
| CA-3 | Styling for the `risk-*` and `overdue-risk` classes is outside this repository, which has no stylesheet. HTML class names are the only styling hook ap_portal provides |

## 7. Open questions (all non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| CMQ-1 | Where are the `overdue-risk` and `risk-*` classes styled? ap_portal has no stylesheet, and the host page is not named (related to DQ-1) | Out of scope for TSK-07; the classes are emitted only | UX lead / Architect |
| CMQ-2 | Which exact version number should the minor step use? | The lane confirms it at FE-04 | Developer |

Carried, unchanged: TQ-2 (`formatAmount` float), TQ-6, TQ-7, TQ-11 / OQ-B10 (padding), OQ-B8 (escaping), OQ-B9 and PQ-P7 (no Risk column or actions), DQ-3 (`TypeError` on contract violation), RAQ-1 (branch name `feature/story`). Each keeps the interim answer used above (feat.design D3; feat.stories; task.repo_access).

## 8. Validation check

| Check | Result | Source |
|---|---|---|
| Existing code read and understood | Pass (section 1) | tool:repo; mcp:codegraph |
| Every component traces to the design and a story AC | Pass (sections 2, 5) | feat.design; feat.stories |
| Within the TSK-07 file budget (`index.js`, `package.json`, `tests/portal.test.js`) | Pass | feat.tasks (TSK-07) |
| Existing signatures kept | Pass | file:ap_portal/CLAUDE.md; feat.design (header "Architecture rule", A5 NF-4, D2) |
| D-2026-044 / L-011 (no float for money) | Consistent (C-FE-4; `formatAmount` not reused) | record:memory |
| Conflict with an approved output | None | — |
| Blocking questions | None | — |
