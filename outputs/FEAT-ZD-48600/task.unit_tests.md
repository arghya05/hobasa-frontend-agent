# task.unit_tests: FEAT-ZD-48600

This file records the unit tests for TSK-07 / STORY-07, "Render the overdue risk table", in repository ap_portal, branch `feature/story`. Step: FE-05 Unit Test Cases (record:brief). Nothing is committed or pushed; the spine commits after evaluation. Statements marked **agent judgement** are this agent's own choices; no input states them as fact.

## 1. Result at a glance

| Item | State | Source |
|---|---|---|
| File changed | Only `tests/portal.test.js` changed, which is this step's write scope (`tests/`). The two existing tests are unchanged, and the new tests come after them | record:brief; tool:repo |
| Code under test | `renderOverdueRisk` and its private helpers `escapeHtml`, `toTwoDecimalString` and `riskClass` in `src/ap_portal/index.js`. No source file was changed in this step | task.code (section 2); tool:repo; mcp:codegraph |
| CI | `node ci.mjs` exits 0, so `passed` is `true`. In `ci.mjs`, `passed` requires compile `true`, no failing test, `coverage >= 90` and no static issues | `node ci.mjs` run in this step; record:brief |
| Coverage gate (≥ 90%) | Met. The CI result recorded with this step reports the unit tests run, none failing, and a coverage of 100%. `ci.mjs` reads this value from the coverage report's `# all files` line-coverage column | `node ci.mjs` run in this step (tool result recorded by the spine) |
| Coverage before this step | 60.32, with lines `26-28 33-39 42-44 50-61` of `src/ap_portal/index.js` uncovered | task.code (section 1.1) |
| Blast radius | `codegraph_explore` reports that `escapeHtml`, `toTwoDecimalString` and `riskClass` each have 1 caller, in `src/ap_portal/index.js`. It reports no covering tests for them. **Agent judgement:** the index does not link the tests' `require('ap_portal')` (resolved through `NODE_PATH=src`) to the file, as task.code section 4 already notes. The CI coverage result is the authority on coverage | mcp:codegraph; task.code (section 4) |
| Affected tests | After `codegraph sync`, `codegraph affected tests/portal.test.js src/ap_portal/index.js` reports `tests/portal.test.js`, which the CI runs | `codegraph affected` run in this step |

## 2. Tests added to `tests/portal.test.js`

All tests use `node:test` and `node:assert`. They load the module with `require('ap_portal')`, as the existing tests do (CI sets `NODE_PATH=src`). Test data is synthetic only: ids `SYN-INV-*` and vendors named "Synthetic Vendor &lt;letter&gt;" (feat.stories TK-08.1, SA-4). A small builder, `row(overrides)`, makes one valid CT-1 row (`id`, `vendor`, `days`, `amount`, `risk`). Each test overrides only the field it checks.

| # | Test name | What it asserts | Acceptance criterion | Source |
|---|---|---|---|---|
| 1 | `AC-07.1: renders the overdue risk table with one row per input row in the given order` | Compares the full markup exactly: `<table class="overdue-risk">`, the header row, one `<tr class="risk-…">` per row with cells id, vendor, days and amount (`class="amount"`), then `</table>`. The input has SYN-INV-001 (31 days) before SYN-INV-002 (42 days). That is fewer days first, the reverse of the ap_aging sort (`days` descending, AC-05.6). The output keeps the input order, so the function does not re-sort | AC-07.1 | feat.stories (AC-07.1, AC-05.6); task.code (FC-6); tool:repo |
| 2 | `AC-07.2: header row has the four headings, no row class and no thead or tbody` | The header is exactly `<tr><th>Invoice</th><th>Vendor</th><th>Days overdue</th><th>Amount</th></tr>` and comes straight after the table tag. "Late" never appears. There is no `<thead>` or `<tbody>` | AC-07.2 [Pending TQ-7] | feat.stories (AC-07.2; section 7, NFR-10 row) |
| 3 | `AC-07.3: each known risk value becomes its row class` | `high`, `medium`, `low` and `unknown` render as `risk-high`, `risk-medium`, `risk-low` and `risk-unknown` | AC-07.3 | feat.stories (AC-07.3) |
| 4 | `AC-07.4: no rows renders an empty string` | `[]`, no argument, `undefined` and `null` each return `''` | AC-07.4 | feat.stories (AC-07.4); task.code (FC-6, section 6) |
| 5 | `AC-07.5: two-decimal amount strings are shown unchanged, beyond Number precision` | `"1250.00"`, `"15.00"` and `"12345678901234567890.12"` are shown unchanged. The test first confirms that `Number()` would change the long string, so that case really is beyond what a JS `Number` can hold | AC-07.5 | feat.stories (AC-07.5); record:memory (D-2026-044, L-011) |
| 6 | `AC-07.6: shorter amount strings are padded to two decimals as strings` | `"980.5"` renders as `980.50`, and `"980"` renders as `980.00` | AC-07.6 [Pending TQ-11, OQ-B10] | feat.stories (AC-07.6); task.code (FC-4) |
| 7 | `AC-07.7: id and vendor text is HTML-escaped` | In `id` and `vendor`, `&`, `<`, `>`, `"` and `'` render as `&amp;`, `&lt;`, `&gt;`, `&quot;` and `&#39;`. An injected `<b>` tag does not appear | AC-07.7 [Pending OQ-B8] | feat.stories (AC-07.7); task.code (FC-3) |
| 8 | `AC-07.8: an unexpected risk value renders as risk-unknown` | `critical`, `HIGH`, `''`, `undefined`, `null` and `"><script>` each render as `risk-unknown`. The markup value never reaches the class attribute | AC-07.8 [Pending TQ-6] | feat.stories (AC-07.8); task.code (FC-1, FC-5) |
| 9 | `AC-07.9: no risk column, risk text or row action` | With the tags stripped, the text has no "risk", "high" or "low". There are exactly 4 `<th>` cells. There is no `button`, `a`, `input` or `form` element, and no "pay", "post", "approve" or "chase" text | AC-07.9 [Pending OQ-B9, PQ-P7] | feat.stories (AC-07.9); tool:repo |
| 10 | `AC-07.10: renderOverdueRisk is exported next to the unchanged exports; helpers stay private` | The export keys are exactly `LABELS`, `VERSION`, `formatAmount`, `renderAgingSummary` and `renderOverdueRisk`, and `renderOverdueRisk` is a function. Together with the two existing tests, which pass unchanged, this covers AC-07.10 | AC-07.10 | feat.stories (AC-07.10); task.code (FC-7) |
| 11 | `rows that are not an array throw TypeError` | A string, a number, `{}`, the array-like `{ length: 1 }` and `true` each throw `TypeError('rows must be an array of overdue rows')` | Coverage case from task.code section 6 | task.code (sections 2, 6); tool:repo |
| 12 | `days that are not an integer throw TypeError` | `30.5`, `'31'`, `undefined`, `null`, `NaN` and `Infinity` each throw `TypeError('days must be an integer')` | Coverage case from task.code section 6 | task.code (sections 2, 6); tool:repo |
| 13 | `amounts that are not a decimal string with at most two decimals throw TypeError` | Each of these throws `TypeError('amount must be a decimal string with at most two decimals')`: an exponent (`'1e3'`, `'1.25e3'`), a thousands separator (`'1,250.00'`), three decimals (`'1250.001'`), `''`, `'.50'`, `'12.'`, `'abc'`, a leading space, the number `1250`, `null` and `undefined` | Coverage case from task.code section 6; AC-07.5 (no float path) | task.code (FC-4, section 6); feat.stories (AC-07.5); tool:repo |

**Agent judgement.** Tests 11 to 13 use more invalid values than task.code section 6 lists, such as `NaN`, `Infinity`, `'.50'`, `'12.'` and the array-like object. These values pin the exact boundaries of `Array.isArray`, `Number.isInteger` and the `DECIMAL_STRING` pattern, so a later change to any of those checks fails a test.

## 3. Coverage by acceptance criterion

| Acceptance criterion | Test(s) |
|---|---|
| AC-07.1 | 1 |
| AC-07.2 | 1, 2 |
| AC-07.3 | 3 |
| AC-07.4 | 4 |
| AC-07.5 | 5, 13 |
| AC-07.6 | 6 |
| AC-07.7 | 7 |
| AC-07.8 | 8 |
| AC-07.9 | 9 |
| AC-07.10 | 10, plus the two existing tests `formats amounts with two decimals` and `renders every bucket in order` |

Sources: feat.stories (STORY-07); tool:repo. The static part of AC-07.10 (no `console.log` or `eval`) is covered by the CI static checks, which are one of the conditions for `passed` in `ci.mjs` (`node ci.mjs` run in this step).

## 4. Not covered here

| Item | Why | Source |
|---|---|---|
| TRD test IDs T-12 to T-16 and the design test list (feat.design B4.5) | TK-07.3 names T-12 to T-16, and task.code section 6 points to the tests in feat.design B4.5. Neither feat.trd nor feat.design is an input of this step. The tests are therefore traced to AC-07.1 to AC-07.10, which feat.stories section 7 maps to T-12 to T-16. See Q-1 | feat.stories (TK-07.3, section 7); task.code (section 6); record:brief |
| End-to-end rendering from the real CT-1 payload (AC-08.3) | STORY-08 belongs to the Testing Agent, not to this lane | feat.stories (STORY-08) |
| Visual styling of `overdue-risk` and the `risk-*` classes | The design library defines no tokens for them. Carried as task.code Q-3 | task.code (section 7) |

## 5. Open questions (none blocks this step)

| ID | Question | Owner |
|---|---|---|
| Q-1 | Should these tests also be checked against the feat.design B4.5 test list and TRD tests T-12 to T-16? Neither file is an input of FE-05, so the tests are traced only to AC-07.1 to AC-07.10 | Developer (step owner) |
| Q-2 | Carried from task.code Q-1: which step sets the next minor version in both `src/ap_portal/index.js` and `package.json` (TK-07.5) before FE-06? No test checks the `VERSION` value, so no test changes when it moves | Developer (step owner) |
| Q-3 | Carried from task.code Q-2: a missing or non-string `id` or `vendor` renders through `String(value)`. No test pins this behaviour, so the Architect's answer will not require undoing a test | Architect |

Also carried, unchanged: the pending markers on AC-07.2, AC-07.6, AC-07.7, AC-07.8 and AC-07.9 (feat.stories). If an answer changes the behaviour, the matching test (2, 6, 7, 8 or 9) changes with it.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Unit tests: 15 run, 0 failing; coverage 100%
