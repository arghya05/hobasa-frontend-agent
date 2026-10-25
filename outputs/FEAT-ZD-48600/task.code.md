# task.code: FEAT-ZD-48600 (Frontend lane)

This file records the code changes on the working branch for STORY-07, "Render the overdue risk table". Step: FE-04 Coding. The spine commits and pushes the changes after the checks and the evaluation; this step commits nothing.

Statements marked **agent judgement** are this agent's own choices. No input states them as fact.

| Field | Value | Source |
|---|---|---|
| Trace ID / story | FEAT-ZD-48600 / STORY-07 (TSK-07) | record:brief; feat.stories (STORY-07); task.components (header) |
| Repository / branch | `ap_portal`, `feature/story` (base `ff761ee`) | task.components (header); record:brief |
| Write scope of this step | `src/ap_portal/`, `docs/`, `README.md` | record:brief |
| Design followed | feat.design B4.1 to B4.4 (HC-8); feat.screens FS-1, FS-2, ST-1 to ST-8; org.design_library | feat.design; feat.screens; org.design_library |
| Component list followed | task.components section 3.1, items 1.1 to 1.9 | task.components |
| Repository files read | `src/ap_portal/index.js`, `CLAUDE.md`, `package.json`, `ci.mjs`, `README.md`, `docs/ap_portal.md` | file:ap_portal/src/ap_portal/index.js; file:ap_portal/CLAUDE.md; file:ap_portal/package.json; file:ap_portal/ci.mjs; file:ap_portal/README.md; file:ap_portal/docs/ap_portal.md |

## 1. Files changed

| File | Status | Change | Source |
|---|---|---|---|
| `src/ap_portal/index.js` | Modified (additive) | Header comment updated; `VERSION` moved from `'2.3.0'` to `'2.4.0'`; new constants `RISK_CLASSES`, `OVERDUE_HEADINGS`, `HTML_ESCAPES` and `DECIMAL_STRING`; new private helpers `escapeHtml`, `toTwoDecimalString` and `riskClass`; new exported `renderOverdueRisk`, added to `module.exports`. The code of `LABELS`, `formatAmount` and `renderAgingSummary` is unchanged | file:ap_portal/src/ap_portal/index.js; tool:repo |
| `docs/ap_portal.md` | New | Module overview: an exports table, and for `renderOverdueRisk` its signature, CT-1 input fields, output, `TypeError` cases, examples and private helpers | file:ap_portal/docs/ap_portal.md |
| `README.md` | Modified | A table of the two render functions and a link to `docs/ap_portal.md` | file:ap_portal/README.md |

`git status --short` lists `README.md` and `src/ap_portal/index.js` as modified, and `docs/` as new and untracked (tool: `git status --short`).

This step's write scope does not include the following files, so they are unchanged (record:brief):
- `tests/portal.test.js`: FE-05 adds the new tests (handoff in section 5).
- `package.json`: `"version"` is still `2.3.0` (file:ap_portal/package.json). See open question CQ-FE-4.

## 2. Implementation (`src/ap_portal/index.js`)

| Item (task.components) | Code | Design / AC trace |
|---|---|---|
| 1.1 Header comment | Now reads "render the aging summary and the overdue risk table returned by the ap_aging service as HTML" | task.components 1.1 (optional; accepted) |
| 1.2 `VERSION` | `'2.4.0'`, a minor step from `2.3.0` | feat.design DD-11; feat.stories TK-07.5; file:ap_portal/CLAUDE.md ("Version: src/ap_portal/index.js") |
| 1.3 `RISK_CLASSES` | `['high', 'medium', 'low', 'unknown']` | feat.design B4.2; org.design_library |
| 1.4 `OVERDUE_HEADINGS` | `['Invoice', 'Vendor', 'Days overdue', 'Amount']`; the header row is built with `map` + `join('')` | feat.design B4.2; org.design_library; AC-07.1, AC-07.2 |
| 1.5 `escapeHtml(value)` | `String(value).replace(/[&<>"']/g, …)`, which looks each character up in `HTML_ESCAPES` (`&amp;`, `&lt;`, `&gt;`, `&quot;`, `&#39;`) | feat.design B4.2, SE-2; AC-07.7 |
| 1.6 `toTwoDecimalString(value)` | Checks that the value is a string, matches it against `DECIMAL_STRING = /^(-?\d+)(?:\.(\d{1,2}))?$/` and pads the fraction with `padEnd(2, '0')`. Any other value throws `TypeError`. Never uses `Number()`, `parseFloat` or `formatAmount` | feat.design B4.2, B4.4, DD-5, DD-8; record:memory D-2026-044, L-011; AC-07.5, AC-07.6 |
| 1.7 `riskClass(risk)` | `RISK_CLASSES.includes(risk) ? \`risk-${risk}\` : 'risk-unknown'` | feat.design B4.2; AC-07.3, AC-07.8 |
| 1.8 `renderOverdueRisk(rows)` | Returns `''` for `undefined` or `null`, throws `TypeError` for a non-array, and returns `''` for `[]`. Otherwise it builds a header row with no class and no `<thead>`/`<tbody>`. For each row, an inline `Number.isInteger(row.days)` check throws `TypeError` on failure; the row then becomes `<tr class="risk-…">` with id, vendor, days and `<td class="amount">`. The result is wrapped in `<table class="overdue-risk">…</table>`. Rows keep the input order: no sorting, no risk text, no Risk column and no row action | feat.design B4.2 steps 1 to 4, B4.4; feat.screens sections 3 to 5; AC-07.1 to AC-07.4, AC-07.9 |
| 1.9 `module.exports` | `{ VERSION, LABELS, formatAmount, renderAgingSummary, renderOverdueRisk }`. The helpers and constants are not exported | feat.design B4.2; AC-07.10; file:ap_portal/CLAUDE.md |

**Comments.** Each new constant (`RISK_CLASSES`, `OVERDUE_HEADINGS`, `HTML_ESCAPES`, `DECIMAL_STRING`) and each new function (`escapeHtml`, `toTwoDecimalString`, `riskClass`, `renderOverdueRisk`) has a `//` comment directly above it. The comments state the purpose, the rule applied (for example D-2026-044 for amounts) and the errors thrown. The comments on the first three constants and on `renderOverdueRisk` also give the story ID STORY-07 (file:ap_portal/src/ap_portal/index.js). No line contains `console.log` or `eval(`, which the CI static check rejects (file:ap_portal/ci.mjs).

**Agent judgement.**
- `HTML_ESCAPES` and `DECIMAL_STRING` are module constants, so the lookup table and the regex are built once rather than on every call. They implement items 1.5 and 1.6 ("a small lookup object", "one regex match") and are not exported (task.components 3.1).
- Each `TypeError` message names its field (`rows`, `days`, `amount`), as feat.design B4.4 requires ("`TypeError` naming the field").

### Expected output against the approved frame (FS-1)

For the FS-1 input rows in feat.screens section 4, the code produces the single-line HTML that feat.screens shows:
- The header comes from `OVERDUE_HEADINGS`.
- Rows are rendered in the given order.
- `"980.50"` and `"1250.00"` match `DECIMAL_STRING` and keep their two decimals.
- `unknown` maps to `risk-unknown`.

This was checked by reading the code. This step may run only `node ci.mjs` and read-only git commands (record:brief), so FE-05 must assert the output in a test (section 5).

## 3. Rules kept

| Rule | How | Source |
|---|---|---|
| Keep existing public signatures; add new functions for new behaviour | Additive only; `formatAmount` and `renderAgingSummary` are untouched | file:ap_portal/CLAUDE.md; file:ap_portal/src/ap_portal/index.js |
| Amounts are never converted to a float | The amount path uses string operations only | record:memory D-2026-044, L-011; org.lessons L-011 |
| No `console.log` / `eval(`; lines at most 140 characters | `node ci.mjs` reports no static issues | file:ap_portal/ci.mjs; tool: `node ci.mjs` |
| "Days overdue", never "Late" | Heading text in `OVERDUE_HEADINGS` | task.components section 6; feat.screens section 6 |
| No styles or tokens invented | Class hooks only; no inline style | feat.screens section 2 ("no tokens and no inline styles"); org.design_library |
| Secrets | None used or referenced; the code does no I/O | task.secrets_scope (by reference only); feat.design NF-10 |

## 4. CI result (`node ci.mjs`)

| Check | Result |
|---|---|
| Compile (`node --check`) | Pass |
| Existing tests (`formats amounts with two decimals`, `renders every bucket in order`) | Both pass unchanged |
| Static checks | No issues |
| Line coverage | **Below the 90% threshold**: no test calls the bodies of the new helpers or of `renderOverdueRisk` yet |
| Overall (`passed`) | **Fails**, on the coverage threshold only |

Sources: tool `node ci.mjs`; file:ap_portal/ci.mjs (threshold `coverage >= 90`); tool:repo (`tests/portal.test.js` test names).

**Why CI fails at the end of FE-04.** `ci.mjs` measures coverage only from `tests/*.test.js` (file:ap_portal/ci.mjs), and this step may not change `tests/` (record:brief). FE-05 adds the unit tests and must bring coverage to 90% or more. Compile, the existing tests and the static checks pass. CI must pass before FE-06 commits and pushes (CQ-FE-5).

## 5. Handoff to FE-05 (tests for `tests/portal.test.js`)

These tests come from task.components section 3.2 and feat.design B4.5. Each `TypeError` branch needs its own test so that coverage reaches 90% or more. Test data stays synthetic (`SYN-INV-*`, "Synthetic Vendor <letter>").

| Test | Covers | AC |
|---|---|---|
| FS-1 rows give the exact FS-1 HTML (feat.screens section 4) | Main path, header, order | AC-07.1, AC-07.2 |
| `high`, `medium`, `low`, `unknown` give the matching `risk-*` class | `riskClass` | AC-07.3 |
| `[]`, `undefined` and `null` give `''` | Empty branches | AC-07.4 |
| `"1250.00"`, `"15.00"` and a digit string longer than a JS `Number` holds are shown unchanged | `toTwoDecimalString` | AC-07.5 |
| `"980.5"` gives `980.50`; `"980"` gives `980.00` | Padding | AC-07.6 |
| A number amount, `"1e3"`, `"1,250.00"` or three decimals throws `TypeError` | Amount errors | DD-8 |
| `<`, `&`, `"` and `'` in id and vendor are escaped | `escapeHtml` | AC-07.7 |
| `risk: 'x" onclick="'` gives `risk-unknown` | Allow-list fallback | AC-07.8 |
| A non-array `rows` (a string) throws `TypeError`; a non-integer `days` throws `TypeError` | Row errors | DD-8 |
| The output has no `Risk` heading, no risk text, and no `<button>` or `<a` | No actions | AC-07.9 |
| `renderOverdueRisk` is exported; the existing tests are unchanged | Exports | AC-07.10 |

## 6. Open questions (non-blocking)

| ID | Question | Interim | Owner (proposed) |
|---|---|---|---|
| CQ-FE-3 | What is the next version number? | `VERSION = '2.4.0'`, a minor step (DD-11) | Developer |
| CQ-FE-4 | `package.json` `"version"` must move with `VERSION` (task.components 3.3), but `package.json` is outside this step's write scope. Who updates it? | Left at `2.3.0`; FE-05 or FE-06 updates it, or the spine widens the scope | Developer |
| CQ-FE-5 | CI line coverage is below 90% after FE-04, because tests are outside this step's scope | FE-05 adds the tests in section 5 and brings coverage to 90% or more | Developer |
| CQ-FE-1, CQ-FE-2, RA-1 (carried) | Who styles the `risk-*` classes; no approved Figma frames; the branch name `feature/story` | As in task.components section 8 | UX lead / Developer |
| TQ-7, TQ-11, OQ-B8, TQ-6, OQ-B9, PQ-P7, DQ-3 (carried) | Interim designs, implemented as specified | As in task.components section 7 | Architect / UX lead |


[Tool result] [ap_portal] Sources compile