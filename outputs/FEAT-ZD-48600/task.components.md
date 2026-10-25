# task.components: FEAT-ZD-48600 (Frontend lane)

This file lists the components and layers of `ap_portal` that the Frontend task must create or update, and how to do it with the fewest lines of code. Step: FE-03, Identify the components/layers to be changed.

Statements marked **agent judgement** are this agent's own reading of the code and inputs; no input states them as fact. This step changes no code. FE-04 does the coding.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-03 Identify the components/layers to be changed | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Task in scope | TSK-07 (STORY-07): render the overdue risk table SC-1 with `renderOverdueRisk` | feat.tasks (sections 1 and 3, TSK-07); task.repo_access (header) |
| Design component | HC-8 `renderOverdueRisk` (new); HC-9 `formatAmount`, `renderAgingSummary` (unchanged) | feat.design (A2, B4.1) |
| Repository and branch | `ap_portal`, branch `feature/story` at `ff761ee`, write access confirmed | task.repo_access (section 1) |
| Inputs read | feat.tasks, feat.stories, feat.prd, feat.trd, feat.design, task.repo_access | record:brief |
| Code read | `src/ap_portal/index.js`, `tests/portal.test.js`, `package.json`, `ci.mjs`, `README.md`, `CLAUDE.md` | tool:repo; file:ap_portal/package.json; file:ap_portal/ci.mjs; file:ap_portal/README.md; file:ap_portal/CLAUDE.md |

## 1. Result

**One source module, one test file and one version field change. No new file, no new layer and no new dependency is needed.**

| # | Component / layer | File | Change | Source |
|---|---|---|---|---|
| 1 | View helper layer (the only runtime layer in ap_portal) | `src/ap_portal/index.js` | Update, additive: two constants, three private helpers, one exported function `renderOverdueRisk`, the `VERSION` step and one more name in `module.exports`; optionally the module header comment (agent judgement, item 1.1) | feat.design (B4.1, B4.2); feat.tasks (TSK-07); tool:repo |
| 2 | Unit tests | `tests/portal.test.js` | Update, additive: new `node:test` tests appended; the two existing tests unchanged | feat.design (B4.5); feat.tasks (TSK-07); tool:repo |
| 3 | Package metadata | `package.json` | Update: `"version"` only, moved together with `VERSION` | feat.tasks (section 2, TSK-07); file:ap_portal/package.json |

Everything else in the repository stays unchanged (section 4).

## 2. What exists today (code read)

| Item | Today | Source |
|---|---|---|
| Runtime | Node.js 22, CommonJS; tests with `node:test` | file:ap_portal/CLAUDE.md; task.repo_access (section 2) |
| Code and test folders | Code in `src/ap_portal/`, tests in `tests/`; version kept in `src/ap_portal/index.js` | file:ap_portal/CLAUDE.md |
| Source files | One: `src/ap_portal/index.js` | task.repo_access (section 2); tool:repo |
| Module contents | `'use strict'`; header comment "AP portal view helpers: render the aging summary returned by the ap_aging service as HTML."; `VERSION = '2.3.0'`; `LABELS`; `formatAmount(value)` (`Number(value \|\| 0).toFixed(2)`); `renderAgingSummary(buckets)` (template literal per row, `rows.join('')`, wrapped in `<table class="aging-summary">`) | tool:repo |
| Exports | `{ VERSION, LABELS, formatAmount, renderAgingSummary }` | tool:repo |
| Tests | `tests/portal.test.js`: `formats amounts with two decimals`; `renders every bucket in order`; the package is loaded with `require('ap_portal')` | tool:repo |
| Package version | `"version": "2.3.0"` in `package.json` | file:ap_portal/package.json |
| CI | `node ci.mjs`: `node --check` on every `.js` file in `src/ap_portal`; tests with `--experimental-test-coverage --test-coverage-include=src/**` and `NODE_PATH=src`; static checks fail a source line matching `console.log` or `eval(` or longer than 140 characters; passes only with coverage >= 90, no failed test and at least one test run | file:ap_portal/ci.mjs |
| Layers not present | No transport or API client, no host page or routing, no stylesheet, no state store. `renderAgingSummary` takes a plain object and returns an HTML string | tool:repo; feat.design (A3 "Transport for CT-1", A1 UI boundary) |

Two findings from the code shape the change:

- `formatAmount` formats through a binary float, which conflicts with D-2026-044. The new code must not call it; the existing helper stays unchanged (feat.trd F-12, TQ-2; record:memory D-2026-044).
- `renderAgingSummary` escapes nothing. Its labels are constants, but the new table renders feed text (`id`, `vendor`), so that pattern must not be copied as it is (feat.trd F-13, R-8).

## 3. Components to create or update, with the minimum-lines approach

### 3.1 `src/ap_portal/index.js` (view helper layer)

Everything below goes into the existing module, next to the existing code, as feat.design B4.1 places it (DD-2). New items are added after `LABELS` and after `renderAgingSummary`; no existing line is rewritten except `VERSION`, the `module.exports` line and, optionally, the header comment.

| # | Item | Kind | Exported | What it does | Minimum-lines approach (agent judgement unless a source says otherwise) | Source |
|---|---|---|---|---|---|---|
| 1.1 | Header comment (line 2) | Update (agent judgement; optional) | — | Mention the overdue risk table next to the aging summary, so the module comment stays accurate | Edit the one existing comment line; no new doc block. Neither feat.design nor feat.tasks lists this edit; it is proposed for the Developer to accept or drop | tool:repo |
| 1.2 | `VERSION` | Update | Yes (already) | Minor version step from `2.3.0`; the lane confirms the next number | Change the string literal only | feat.tasks (section 2, TSK-07); feat.trd (T-A6) |
| 1.3 | `RISK_CLASSES` | New constant | No | Allow-list `['high', 'medium', 'low', 'unknown']` | One line | feat.design (B4.2, B4.3) |
| 1.4 | `OVERDUE_HEADINGS` | New constant | No | `['Invoice', 'Vendor', 'Days overdue', 'Amount']` in order | One line; the header row is built from it with `map` + `join('')`, as `renderAgingSummary` builds its rows | feat.design (B4.2); feat.prd (AC-19); tool:repo |
| 1.5 | `escapeHtml(value)` | New private helper | No | Escapes `&`, `<`, `>`, `"`, `'` | A single `String(value).replace(/[&<>"']/g, ...)` with a small lookup object | feat.design (B4.2, SE-2); feat.trd (IF-7.7) |
| 1.6 | `toTwoDecimalString(value)` | New private helper | No | String-only amount: must be a string matching `^-?\d+(\.\d{1,2})?$`, padded to two decimals as a string (`"980.5"` → `980.50`); otherwise `TypeError`. Never `Number()`, `parseFloat` or `formatAmount` | One type check plus one regex match with capture groups; pad the fraction with `padEnd(2, '0')`. No numeric conversion at any point | feat.design (B4.2, B4.4, DD-5, DD-8); feat.trd (IF-7.5); feat.tasks (TSK-07 implementation notes); record:memory (D-2026-044, L-011) |
| 1.7 | `riskClass(risk)` | New private helper | No | `risk-<risk>` for a known value, else `risk-unknown` | One expression with `RISK_CLASSES.includes(risk)` | feat.design (B4.2); feat.trd (IF-7.4) |
| 1.8 | `renderOverdueRisk(rows)` | New function | Yes | `''` for `undefined`, `null` or `[]`; `TypeError` for any other non-array; otherwise `<table class="overdue-risk">` + plain header row (no class, no `<thead>`/`<tbody>`) + one `<tr class="risk-…">` per row in the given order with cells id, vendor, days, amount (`class="amount"`) + `</table>`; `TypeError` when `days` is not an integer | Same style as `renderAgingSummary`: template literals, `rows.map(...)`, `join('')`. The `days` check is an inline `Number.isInteger` test inside the row mapper, not another helper. No sorting, no risk text, no Risk column, no row actions | feat.design (B4.2 algorithm steps 1 to 4, B4.4); feat.trd (IF-7.1 to IF-7.8); feat.tasks (TSK-07); feat.stories (AC-07.1 to AC-07.9) |
| 1.9 | `module.exports` | Update | — | Add `renderOverdueRisk` after the existing four names | Append one name to the existing line; the helpers and constants stay private, so the public surface grows by one function only | feat.design (B4.2); feat.stories (AC-07.10); file:ap_portal/CLAUDE.md ("Keep existing public signatures; add new functions for new behaviour.") |
| — | `LABELS`, `formatAmount`, `renderAgingSummary` | Unchanged | Yes (already) | — | Not touched and not reused by the new code | feat.design (HC-9, B4.1); feat.tasks (TSK-07 "Must not change"); feat.trd (C-9) |

**Why no new source file (agent judgement).** feat.design B4.1 and feat.tasks TSK-07 put all of this in `src/ap_portal/index.js`; a new module would be outside the task's file budget. CI also compiles every `.js` file in `src/ap_portal` and measures coverage over `src/**` (file:ap_portal/ci.mjs), so a second file would add an export line, a `require` and its own coverage to keep up, with no gain.

**Why the helpers stay private (agent judgement).** Exporting them would add public signatures that later changes must keep (file:ap_portal/CLAUDE.md), and feat.design B4.2 exports only `renderOverdueRisk`. The helpers are covered through `renderOverdueRisk` in the tests.

**Kept as named helpers although they could be inlined (agent judgement).** `riskClass` and `escapeHtml` could be inlined to save a few lines, but feat.design B4.1 and feat.tasks TSK-07 name them as components. This file follows the approved design rather than trimming it.

### 3.2 `tests/portal.test.js` (unit tests)

Append tests; do not edit the two existing tests. Every error branch needs its own test, because coverage is measured over `src/**` and must stay at 90 or above (file:ap_portal/ci.mjs; feat.tasks section 2, CI bar).

| Test (names from feat.design B4.5; `null` and the extra `TypeError` cases from feat.tasks TSK-07) | Covers in `index.js` | Acceptance criteria |
|---|---|---|
| `renders overdue risk table` | 1.4, 1.8 main path | AC-07.1, AC-07.2 |
| `maps every risk value to its class` | 1.3, 1.7 | AC-07.3 |
| `returns empty string for no rows` (`[]`, `undefined`, `null`) | 1.8 empty branch | AC-07.4 |
| `shows amount strings unchanged` (`"1250.00"`, `"15.00"`, a string with more digits than a JS `Number` holds) | 1.6 | AC-07.5 |
| `pads short amounts as strings` (`"980.5"` → `980.50`) | 1.6 padding | AC-07.6 |
| `rejects non-string amounts`; amount with an exponent, a thousands separator or three decimals → `TypeError` | 1.6 error branches | DD-8 (feat.design) |
| `escapes id and vendor` | 1.5 | AC-07.7 |
| `unexpected risk renders risk-unknown` | 1.7 fallback | AC-07.8 |
| Non-array `rows` → `TypeError`; non-integer `days` → `TypeError` | 1.8 error branches | DD-8 (feat.design) |
| No Risk column, no risk text, no row action in the output | 1.8 | AC-07.9 |
| Existing tests unchanged; `renderOverdueRisk` exported next to the existing exports | 1.9 | AC-07.10 |

Sources: feat.design (B4.5); feat.tasks (TSK-07 "Tests to add"); feat.stories (STORY-07 acceptance criteria).

Minimum-lines approach for the tests (agent judgement): one shared WF-1-style fixture array of CT-1 rows reused across tests, and `assert.throws(..., TypeError)` for the error branches, so each branch is one assertion line. Test data stays synthetic (`SYN-INV-*`, "Synthetic Vendor <letter>") (feat.design SE-4).

### 3.3 `package.json` (package metadata)

| Field | Change | Source |
|---|---|---|
| `"version"` | Same minor step as `VERSION` in `index.js`; nothing else in the file changes | feat.tasks (section 2, TSK-07 "Done when"); file:ap_portal/package.json |

## 4. Components and layers not changed

| Component / layer | Why not | Source |
|---|---|---|
| `LABELS`, `formatAmount`, `renderAgingSummary` and their tests | Must not change; the `formatAmount` float issue stays open as TQ-2 | feat.tasks (TSK-07); feat.trd (C-9, F-12, TQ-2) |
| `ci.mjs` | CI already compiles, tests with coverage and runs the static checks the task needs | file:ap_portal/ci.mjs |
| `README.md`, `CLAUDE.md` | Not in the task's file budget; the README holds only the package description and the CI command | feat.tasks (TSK-07 files in budget); file:ap_portal/README.md; file:ap_portal/CLAUDE.md |
| Transport or API client between ap_aging and ap_portal | Not added; the consumer parses CT-1 and passes the array. A transport would re-trigger DES-01 | feat.design (A3, DD-3, A8) |
| Host page, navigation, filters, row actions | Out of this design | feat.design (A1 UI boundary); feat.trd (IF-7.8) |
| Stylesheet for the `risk-*` classes | No stylesheet exists in the repository and none is in the task's file budget; the function only sets the class | tool:repo; feat.tasks (TSK-07 files in budget) |
| Other repositories (ap_aging, ap_pipeline, ap_forecast) and the end-to-end harness | Other lanes (TSK-01 to TSK-06) and the Testing Agent (TSK-08) | feat.tasks (section 1) |
| Dependencies | None added: Node.js 22 CommonJS only | feat.design (DD-1) |

## 5. Contract the component consumes (CT-1)

`renderOverdueRisk` is built against the CT-1 field list only, not the ap_aging code, so it can be built in parallel with TSK-06.

| Field | Type | How the component uses it | Source |
|---|---|---|---|
| `id` | string | Escaped, first cell | feat.design (A3 CT-1, B4.2) |
| `vendor` | string | Escaped, second cell | feat.design (A3 CT-1, B4.2) |
| `days` | integer | Integer text, third cell; `TypeError` if not an integer | feat.design (A3 CT-1, B4.4) |
| `amount` | two-decimal string | String-only display, `class="amount"` cell | feat.design (A3 CT-1, B4.2); feat.trd (IF-7.5) |
| `risk` | `high`, `medium`, `low` or `unknown` | Row class only, through the allow-list | feat.design (A3 CT-1, B4.2); feat.trd (IF-7.4) |

The input is the array from `JSON.parse` of the ap_aging payload; an empty payload is `[]` (feat.design A3, B4.3).

## 6. Rules the change must keep

| Rule | Source |
|---|---|
| Keep existing public signatures; add new functions for new behaviour | file:ap_portal/CLAUDE.md; feat.design (header, architecture rule) |
| Monetary amounts are never converted to a float: no `Number()`, `parseFloat` or `formatAmount` on the amount | record:memory (D-2026-044, L-011); feat.trd (IF-7.5) |
| No source line matching `console.log` or `eval(` (comments included); no line longer than 140 characters | file:ap_portal/ci.mjs; feat.tasks (section 2, CI bar) |
| Coverage of at least 90% with every error branch tested | file:ap_portal/ci.mjs; file:ap_portal/CLAUDE.md; feat.tasks (section 2, CI bar) |
| Terminology: "Days overdue", never "Late" | feat.prd (NFR-10); feat.design (NF-12) |
| Commit messages start with the story ID (`STORY-07`); the task ID may follow | feat.tasks (section 2); record:memory (conventions) |

## 7. Pending items that touch these components (carried, non-blocking)

Each is built on its interim design; if an answer differs, only the named item changes.

| ID | Affects | Interim design used | Source |
|---|---|---|---|
| TQ-7 | 1.4, 1.8 header row | Plain `<tr><th>…</th></tr>`, no `<thead>`/`<tbody>` | feat.trd (IF-7.2, TQ-7); feat.stories (AC-07.2) |
| TQ-11, OQ-B10 | 1.6 | Pad short amounts as strings | feat.trd (IF-7.5, TQ-11); feat.stories (AC-07.6) |
| OQ-B8 | 1.5 | Escape `id` and `vendor` | feat.trd (IF-7.7); feat.stories (AC-07.7) |
| TQ-6 | 1.3, 1.7 | Unknown `risk` renders `risk-unknown` | feat.trd (IF-7.4, TQ-6); feat.stories (AC-07.8) |
| OQ-B9, PQ-P7 | 1.8 | No Risk column, no risk text, no row actions | feat.trd (IF-7.8); feat.stories (AC-07.9) |
| DQ-3 | 1.6, 1.8 | Throw `TypeError` on a CT-1 violation | feat.design (DD-8, D3) |
| TQ-2 | `formatAmount` (not changed) | Kept as is; not used by the new code | feat.trd (F-12, TQ-2) |

## 8. Open questions (all non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| CQ-FE-1 | The `risk-*` row classes need styles to be visible to the AP manager, but no stylesheet exists in `ap_portal` and none is in the task's file budget. Who owns the styles? | The function sets the class only; styling stays with the host page, outside this task | UX lead / Developer |
| CQ-FE-2 | No approved Figma frames exist for SC-1 (Figma is not connected); the screens are text frames. | Follow feat.design B4.2 and, in FE-04, feat.screens | UX lead |
| CQ-FE-3 | Exact next version number for `VERSION` and `"version"` | A minor step, confirmed by the lane in FE-04 | Developer |
| RA-1 (carried) | The working branch is `feature/story`, not `feature/STORY-07`, while the convention is `feature/<story>` | Use `feature/story`; commit messages start with `STORY-07` | Developer |

Sources: tool:repo; feat.tasks (TSK-07, section 2); feat.stories (STORY-07 frame links, PQ-P6); feat.design (DQ-5); task.repo_access (RA-1); file:ap_portal/CLAUDE.md (branch convention).

## 9. Validation check

| Check | Result | Source |
|---|---|---|
| Existing code read before listing changes | Pass: every file in the repository's working tree was read | tool:repo; file:ap_portal/ci.mjs; file:ap_portal/package.json; file:ap_portal/README.md; file:ap_portal/CLAUDE.md |
| Changes stay inside the TSK-07 file budget | Pass: `src/ap_portal/index.js`, `package.json` (version only), `tests/portal.test.js` | feat.tasks (TSK-07) |
| Every listed component traces to the design | Pass: items 1.2 to 1.9 map to feat.design B4.1 and B4.2; item 1.1 (header comment) is agent judgement | feat.design (B4) |
| Every STORY-07 acceptance criterion has a component and a test | Pass: AC-07.1 to AC-07.10 in section 3.2 | feat.stories (STORY-07) |
| No existing signature changed | Pass: only additions plus the `VERSION` value | file:ap_portal/CLAUDE.md; feat.design (B4.1) |
| Decision log and lessons | Consistent: D-2026-044 and L-011 (no float on amounts) | record:memory |
| Conflict with an approved output | None | feat.design; feat.tasks; feat.stories |
| Blocking questions | None | — |
