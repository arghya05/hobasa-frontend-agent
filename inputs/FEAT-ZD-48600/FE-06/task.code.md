<!-- input task.code for FE-06: from hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.code.md at commit 7b73e9c, produced by FE-04 -->
# task.code: FEAT-ZD-48600

This file records the code changes for TSK-07 / STORY-07, "Render the overdue risk table", in repository ap_portal, branch `feature/story`. Step: FE-04 Coding (record:brief). Nothing is committed or pushed; the spine commits after evaluation. Statements marked **agent judgement** are this agent's own choices; no input states them as fact.

## 1. Result at a glance

| Item | State | Source |
|---|---|---|
| Files changed | `src/ap_portal/index.js` (modified), `README.md` (modified) and `docs/ap_portal.md` (new). All three are in this step's write scope (`src/ap_portal/`, `docs/`, `README.md`). `git status` shows no other change | record:brief; file:ap_portal/src/ap_portal/index.js |
| Compile | CI reports `compile` as `true` | `node ci.mjs` run in this step; file:ap_portal/ci.mjs |
| Existing tests | `formats amounts with two decimals` and `renders every bucket in order` both pass. Neither test was changed | `node ci.mjs` run in this step; tool:repo |
| Static checks | `static_issues` is empty: no `console.log`, no `eval(` and no line longer than 140 characters | `node ci.mjs` run in this step; file:ap_portal/ci.mjs |
| Coverage gate | Not applied in this step, by owner decision. The owner accepted FE-04 without the coverage gate. FE-05 writes the new tests, and the full CI, including the 90% gate, must pass before the commit step | record:owner_answer; file:ap_portal/ci.mjs |
| Existing behaviour | `VERSION`, `LABELS`, `formatAmount` and `renderAgingSummary` are unchanged | file:ap_portal/src/ap_portal/index.js; mcp:codegraph |

### 1.1 CI output (last stdout line of `node ci.mjs`, run in this step)

```json
{"compile":true,"tests_run":2,"failures":0,"errors":0,"coverage":60.32,"static_issues":[],"passed":false}
```

The `test_output` and `coverage_report` fields are left out above. In the coverage report, the uncovered lines of `src/ap_portal/index.js` are `26-28 33-39 42-44 50-61`. These are the bodies of the four new functions, and no test calls them yet. `passed` is `false` only because the `coverage >= 90` condition fails (file:ap_portal/ci.mjs, line 17). The other conditions in that check all hold.

## 2. What changed in `src/ap_portal/index.js`

The change is additive. No existing line changes except `module.exports`, which now also exports `renderOverdueRisk`. The new code goes between `renderAgingSummary` and `module.exports`. This follows the repository rule "Keep existing public signatures; add new functions for new behaviour" (file:ap_portal/CLAUDE.md) and components FC-1 to FC-7 (task.components).

| Component | Code | Implements | Source |
|---|---|---|---|
| FC-1 `RISK_CLASSES` | `['high', 'medium', 'low', 'unknown']` | Allow-list for the row class | file:ap_portal/src/ap_portal/index.js; task.components; feat.design (B4.2); org.design_library |
| FC-2 `OVERDUE_HEADINGS` | `['Invoice', 'Vendor', 'Days overdue', 'Amount']`. The header row is built by mapping over this constant | AC-07.1, AC-07.2; "Days overdue", never "Late" | file:ap_portal/src/ap_portal/index.js; task.components; feat.stories (AC-07.1, AC-07.2); feat.screens (section 6); org.design_library |
| `HTML_ESCAPES`, `DECIMAL_STRING` | `HTML_ESCAPES` is the character map used by `escapeHtml`. `DECIMAL_STRING` is the pattern `^-?\d+(\.\d{1,2})?$` used by `toTwoDecimalString`. Neither is exported | Supporting constants. **Agent judgement:** task.components plans two constants (FC-1, FC-2). These two extra constants hold the map and the pattern that feat.design B4.2 puts inside the helpers, so each is defined once | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2); task.components (section 2) |
| FC-3 `escapeHtml(value)` (private) | Applies `String(value)`, then replaces `&`, `<`, `>`, `"` and `'` with `&amp;`, `&lt;`, `&gt;`, `&quot;` and `&#39;` | AC-07.7 | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2, SE-2); feat.stories (AC-07.7) |
| FC-4 `toTwoDecimalString(value)` (private) | Accepts only a string that matches `DECIMAL_STRING` and pads the fraction to two digits as a string. Anything else throws `TypeError('amount must be a decimal string with at most two decimals')`; the message names the field, as B4.4 requires. It never calls `Number()`, `parseFloat` or `formatAmount` | AC-07.5, AC-07.6; D-2026-044; L-011 | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2, B4.4, DD-8); feat.stories (AC-07.5, AC-07.6); org.lessons; record:memory |
| FC-5 `riskClass(risk)` (private) | Returns `risk-<risk>` for a value on the allow-list, else `risk-unknown` | AC-07.3, AC-07.8 | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2); feat.stories (AC-07.3, AC-07.8) |
| FC-6 `renderOverdueRisk(rows)` (exported) | Returns `''` for `undefined`, `null` or `[]`. Throws `TypeError` when `rows` is any other non-array value, or when a `days` value fails `Number.isInteger`. Otherwise returns `<table class="overdue-risk">`, the header row, one `<tr class="risk-…">` per row in the given order with cells id, vendor, days and amount (`class="amount"`), then `</table>`. It adds no `<thead>`/`<tbody>`, does not re-sort, shows no risk text and has no row action | AC-07.1 to AC-07.4, AC-07.9; SC-1 frames FS-1 and FS-2 | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2 algorithm, B4.4); feat.screens (sections 3, 4); feat.stories (STORY-07) |
| FC-7 `module.exports` | `{ VERSION, LABELS, formatAmount, renderAgingSummary, renderOverdueRisk }`. The helpers stay private | AC-07.10 | file:ap_portal/src/ap_portal/index.js; feat.design (B4.2); feat.stories (AC-07.10) |

**Comments.** A `//` comment sits directly above each of the four new constants (`RISK_CLASSES`, `OVERDUE_HEADINGS`, `HTML_ESCAPES`, `DECIMAL_STRING`) and each of the four new functions (`escapeHtml`, `toTwoDecimalString`, `riskClass`, `renderOverdueRisk`). Between them, the comments state:

- the trace `FEAT-ZD-48600 / STORY-07`;
- the money rule (D-2026-044);
- the reason for escaping;
- the accepted amount format;
- the return values;
- every `TypeError` path.

Source: file:ap_portal/src/ap_portal/index.js.

**Error messages.** feat.design B4.4 requires the amount `TypeError` to name the field. It does not set the messages for `rows` and `days`. The messages `'rows must be an array of overdue rows'` and `'days must be an integer'` are **agent judgement** (file:ap_portal/src/ap_portal/index.js; feat.design B4.4).

### 2.1 Match with the approved screen

This match was checked by reading the code against the frames. No test runs it yet.

- **FS-1 (populated).** For the FS-1 input rows, `renderOverdueRisk` produces the markup in feat.screens section 4, on one line: the header row `<tr><th>Invoice</th><th>Vendor</th><th>Days overdue</th><th>Amount</th></tr>`, then one `<tr class="risk-…"><td>id</td><td>vendor</td><td>days</td><td class="amount">amount</td></tr>` per row, in input order.
- **FS-2 (empty).** For `[]`, it returns `''`.
- **Defensive states.**
  - ST-6: an unexpected risk value renders as `risk-unknown`.
  - ST-7: markup in the text renders as escaped text.
  - ST-8: the amount `"980.5"` renders as `980.50`.

The FE-05 tests listed in feat.design B4.5 will confirm all of this (file:ap_portal/src/ap_portal/index.js; feat.screens; feat.design B4.5).

**Design library.** The component emits only the class hooks `overdue-risk`, `risk-high`, `risk-medium`, `risk-low`, `risk-unknown` and `amount`. It sets no inline style and uses no tokens, because the design library defines none (org.design_library; feat.screens section 2).

## 3. Documentation

| File | Content | Source |
|---|---|---|
| `README.md` | A function table that includes `renderOverdueRisk(rows)`, a short description of its input and behaviour, and a link to the module doc | record:brief; file:ap_portal/README.md |
| `docs/ap_portal.md` (new) | A module overview and the export table. For `renderOverdueRisk`: the signature, the CT-1 input fields, the output, the error table, the money and escaping rules, a synthetic example (SYN-INV-004, SYN-INV-002) and the styling hooks. It also covers the private helpers and constants | feat.design (A3 CT-1, B4); feat.screens; record:brief; file:ap_portal/docs/ap_portal.md |

## 4. Blast radius

| Check | Result | Source |
|---|---|---|
| `codegraph_explore` on the new and changed symbols | `HTML_ESCAPES`, `DECIMAL_STRING`, `escapeHtml` and `toTwoDecimalString` each have 1 caller, all inside `src/ap_portal/index.js`. `HTML_ESCAPES` is used only by `escapeHtml`. `DECIMAL_STRING` is used only by `toTwoDecimalString`. Both helpers are used only by `renderOverdueRisk`. `formatAmount` is called only by `renderAgingSummary`, and the new code does not call it | mcp:codegraph |
| `codegraph affected src/ap_portal/index.js` (after `codegraph sync`) | No affected test files reported. **Agent judgement:** the tests load the module by package name (`require('ap_portal')`), which CI resolves through `NODE_PATH=src`, and the index does not link that import to the file. So I ran the full CI, and both existing tests pass | mcp:codegraph; tool:repo; file:ap_portal/ci.mjs |

## 5. Not changed in this step

| Item | Why | Source |
|---|---|---|
| `VERSION` (`'2.3.0'`) and the `package.json` `"version"` (`2.3.0`) (FC-8, FC-9) | DD-11 needs a minor version step in both files together. `package.json` is outside this step's write scope, and changing only `VERSION` would split the two, so both stay at `2.3.0` and still match. Proposal (TK-07.5): take one minor step from `2.3.0` in `index.js` and `package.json` together, in a step whose scope includes `package.json`. See Q-1 | task.components (FC-8, FC-9); feat.design (DD-11); feat.stories (TK-07.5); file:ap_portal/package.json; record:brief |
| `tests/portal.test.js` (FC-10) | Written in FE-05 | record:owner_answer; task.components (FC-10) |
| `LABELS`, `formatAmount`, `renderAgingSummary`, `ci.mjs` | Must not change | task.components (section 3); feat.design (B4.1) |

## 6. Hand-over to FE-05

FE-05 adds the eight tests in feat.design B4.5 and the coverage cases in task.components FC-10:

- non-array `rows` → `TypeError`
- non-integer `days` → `TypeError`
- `null` → `''`
- an amount with an exponent, a thousands separator or three decimals → `TypeError`

These tests cover the uncovered lines listed in section 1.1, so `node ci.mjs` reports `passed: true` before the commit step (record:owner_answer; feat.design B4.5; task.components FC-10; feat.stories TK-07.3, TK-07.4).

## 7. Open questions (none blocks this step)

| ID | Question | Owner |
|---|---|---|
| Q-1 | Version step (FC-8, FC-9; DD-11; TK-07.5): `package.json` is outside FE-04's write scope. Which step sets the next minor version in both `src/ap_portal/index.js` and `package.json`, and who confirms the number? This must be settled before the commit step (FE-06) | Developer (step owner) |
| Q-2 | Carried from FE-03 (CQ-FE-1): as feat.design B4.2 specifies, a missing or non-string `id` or `vendor` renders through `String(value)`, for example as the text `undefined`. Should it throw `TypeError` instead, as `days` and `amount` do (DD-8)? | Architect |
| Q-3 | Carried from FE-03 (CQ-FE-2): who styles `overdue-risk` and the `risk-*` classes? The design library defines no tokens for them | UX lead |

Also carried, unchanged: the pending markers on AC-07.2, AC-07.6, AC-07.7, AC-07.8 and AC-07.9 (feat.stories).


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Sources compile
