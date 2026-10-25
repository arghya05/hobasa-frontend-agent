# Evaluate step FE-05: Unit Test Cases for FEAT-ZD-48600

- Workbook action: Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage ²
- Workbook outcome: Unit testing completed

## Outputs to evaluate
- `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`

## Inputs the agent was given
- task.code: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-05/task.code.md`
- feat.stories: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-05/feat.stories.md`

## Tool results the spine recorded from the real tools in this step (a valid source; the section 'Tool results' at the end of an output file is written by the spine, not the agent)


[Tool result] [ap_portal] Unit tests: 15 run, 0 failing; coverage 100%

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `hobasa-frontend-agent/.claude/skills/frontend-steps/SKILL.md`
- `hobasa-frontend-agent/CLAUDE.md`
- `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`
- `ap-aging-sample/CLAUDE.md`
- `ap-forecast-sample/CLAUDE.md`
- `ap-pipeline-sample/CLAUDE.md`
- `ap-portal-sample/CLAUDE.md`
- `ap-portal-sample/ci.mjs`
- `ap-portal-sample/tests/portal.test.js`
- `hobasa-agentic-sdlc/CLAUDE.md`
- `hobasa-agentic-sdlc/features/FEAT-ZD-48600/JOURNEY.md`
- `hobasa-agentic-sdlc/memory/MEMORY.md`
- `final_document/hobasa-claude-code-plugin/skills/frontend-steps/SKILL.md`
- `final_document/hobasa-claude-code-plugin/skills/grounded-output/SKILL.md`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

Your previous result for this step was refused. Fix every point, rewrite the output files in full, and return the structured result again. The outputs describe the work itself: never mention earlier attempts, refusals or this review in them.

## Refusal reasons
G3 numbers ['15', '100'] in "After the new tests, node ci.mjs reports compile true, 15 tests run, 0 failures, 0 errors, coverage " do not appear in tool:repo; file:ap_portal/ci.mjs
G2 claim cites mcp:codegraph but that connector was not used in this session

## Step brief (unchanged except the attempt number)
# Step FE-05: Unit Test Cases (attempt 2) for FEAT-ZD-48600

- Workbook trigger: Code completion
- Workbook action: Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage ²
- Workbook outcome: Unit testing completed
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.code (Code changes on the working branch): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-05/task.code.md` (from hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.code.md at commit 7b73e9c)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-05/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.unit_tests (Unit tests and coverage (≥90%)): write `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`

## Code
Work in repository `ap-portal-sample` (Node.js 22 (CommonJS, node:test)). You may change only: tests/. Understand before you change: the repository is indexed with CodeGraph. Before editing, use it (MCP tools codegraph_explore and codegraph_node with projectPath `ap-portal-sample`, or `codegraph explore <symbols>`, `codegraph impact <symbol>`, `codegraph callers <symbol>`) to read the exact source of what you change, its callers and callees and the blast radius. After editing, run `codegraph sync` and `codegraph affected <changed files>` to find the tests your change touches, and run them through the CI. Shell commands allowed in this step: `node ci.mjs`, `git status`, `git diff`, `git log`, `codegraph explore`, `codegraph node`, `codegraph callers`, `codegraph callees`, `codegraph impact`, `codegraph affected`, `codegraph query`, `codegraph files`, `codegraph status`, `codegraph sync`. Run the CI yourself with `node ci.mjs` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does.

## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier, ci-runner (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:npm (npm package metadata: versions, licence (GET /<package>)), mcp:codegraph (Code intelligence over the repository: a symbol's source, its callers and callees, the blast radius of a change and the tests it affects (codegraph_explore, codegraph_node and related tools; pass projectPath = the repository checkout)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo)
--- ap_portal/src/ap_portal/index.js ---
'use strict';
// AP portal view helpers: render the aging summary returned by the ap_aging service as HTML.
const VERSION = '2.3.0';
const LABELS = { current: 'Current', '1-30': '1-30 days', '31-60': '31-60 days', '61-90': '61-90 days', '90+': 'Over 90 days' };

function formatAmount(value) {
  return Number(value || 0).toFixed(2);
}

function renderAgingSummary(buckets) {
  const rows = Object.keys(LABELS).map((k) => `<tr><td>${LABELS[k]}</td><td class="amount">${formatAmount(buckets[k])}</td></tr>`);
  return `<table class="aging-summary">${rows.join('')}</table>`;
}

// FEAT-ZD-48600 / STORY-07: overdue risk table (SC-1) rendered from the CT-1 rows that ap_aging serialises.
// Allow-list of row risk values; anything else renders as risk-unknown.
const RISK_CLASSES = ['high', 'medium', 'low', 'unknown'];
// Column headings, in order. Always "Days overdue", never "Late".
const OVERDUE_HEADINGS = ['Invoice', 'Vendor', 'Days overdue', 'Amount'];
// HTML entity for each character escapeHtml replaces.
const HTML_ESCAPES = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' };
// CT-1 amount: optional minus, digits, then at most two decimals. No exponent, no thousands separator.
const DECIMAL_STRING = /^-?\d+(\.\d{1,2})?$/;

// Escape &, <, >, " and ' so feed text cannot inject markup.
function escapeHtml(value) {
  return String(value).replace(/[&<>"']/g, (ch) => HTML_ESCAPES[ch]);
}

// Two-decimal display done on the string only, so no amount ever passes through a binary float
// (D-2026-044). Never uses Number(), parseFloat or formatAmount. "980.5" -> "980.50", "980" -> "980.00".
// Throws TypeError for a non-string or a string with an exponent, separator or more than two decimals.
function toTwoDecimalString(value) {
  if (typeof value !== 'string' || !DECIMAL_STRING.test(value)) {
    throw new TypeError('amount must be a decimal string with at most two decimals');
  }
  const [whole, fraction = ''] = value.split('.');
  return `${whole}.${fraction.padEnd(2, '0')}`;
}

// Row class for a risk value: 'risk-<risk>' for a known value, else 'risk-unknown'.
function riskClass(risk) {
  return RISK_CLASSES.includes(risk) ? `risk-${risk}` : 'risk-unknown';
}

// Render CT-1 rows ({id, vendor, days, amount, risk}) as <table class="overdue-risk">.
// Returns '' when rows is undefined, null or []. Rows keep the given order (ap_aging sorts them).
// Risk is shown only as the row class; there is no risk text and no row action (advisory only).
// Throws TypeError when rows is not an array, days is not an integer or amount is not a decimal string.
function renderOverdueRisk(rows) {
  if (rows === undefined || rows === null) return '';
  if (!Array.isArray(rows)) throw new TypeError('rows must be an array of overdue rows');
  if (rows.length === 0) return '';
  const header = `<tr>${OVERDUE_HEADINGS.map((h) => `<th>${h}</th>`).join('')}</tr>`;
  const body = rows.map((row) => {
    if (!Number.isInteger(row.days)) throw new TypeError('days must be an integer');
    return `<tr class="${riskClass(row.risk)}"><td>${escapeHtml(row.id)}</td><td>${escapeHtml(row.vendor)}</td>`
      + `<td>${row.days}</td><td class="amount">${toTwoDecimalString(row.amount)}</td></tr>`;
  });
  return `<table class="overdue-risk">${header}${body.join('')}</table>`;
}

module.exports = { VERSION, LABELS, formatAmount, renderAgingSummary, renderOverdueRisk };

--- ap_portal/tests/portal.test.js ---
'use strict';
const test = require('node:test');
const assert = require('node:assert');
const portal = require('ap_portal');

test('formats amounts with two decimals', () => {
  assert.strictEqual(portal.formatAmount(12), '12.00');
  assert.strictEqual(portal.formatAmount(undefined), '0.00');
});

test('renders every bucket in order', () => {
  const html = portal.renderAgingSummary({ current: 10, '90+': 5 });
  assert.ok(html.startsWith('<table class="aging-summary">'));
  assert.ok(html.indexOf('Current') < html.indexOf('Over 90 days'));
  assert.ok(html.includes('<td class="amount">5.00</td>'));
});

// FEAT-ZD-48600 / STORY-07: renderOverdueRisk (AC-07.1 to AC-07.10). Synthetic data only.
const HEADER = '<tr><th>Invoice</th><th>Vendor</th><th>Days overdue</th><th>Amount</th></tr>';
const row = (overrides) => ({ id: 'SYN-INV-001', vendor: 'Synthetic Vendor A', days: 31, amount: '15.00', risk: 'high', ...overrides });
const AMOUNT_ERROR = { name: 'TypeError', message: 'amount must be a decimal string with at most two decimals' };
const DAYS_ERROR = { name: 'TypeError', message: 'days must be an integer' };
const ROWS_ERROR = { name: 'TypeError', message: 'rows must be an array of overdue rows' };

test('AC-07.1: renders the overdue risk table with one row per input row in the given order', () => {
  const html = portal.renderOverdueRisk([
    row({ id: 'SYN-INV-002', vendor: 'Synthetic Vendor B', days: 42, amount: '1250.00', risk: 'medium' }),
    row({ id: 'SYN-INV-001', vendor: 'Synthetic Vendor A', days: 31, amount: '15.00', risk: 'high' }),
  ]);
  assert.strictEqual(html, '<table class="overdue-risk">' + HEADER
    + '<tr class="risk-medium"><td>SYN-INV-002</td><td>Synthetic Vendor B</td><td>42</td><td class="amount">1250.00</td></tr>'
    + '<tr class="risk-high"><td>SYN-INV-001</td><td>Synthetic Vendor A</td><td>31</td><td class="amount">15.00</td></tr>'
    + '</table>');
});

test('AC-07.2: header row has the four headings, no row class and no thead or tbody', () => {
  const html = portal.renderOverdueRisk([row()]);
  assert.ok(html.startsWith(`<table class="overdue-risk">${HEADER}`));
  assert.ok(html.includes('Days overdue'));
  assert.ok(!html.includes('Late'));
  assert.ok(!html.includes('<thead') && !html.includes('<tbody'));
});

test('AC-07.3: each known risk value becomes its row class', () => {
  for (const risk of ['high', 'medium', 'low', 'unknown']) {
    assert.ok(portal.renderOverdueRisk([row({ risk })]).includes(`<tr class="risk-${risk}">`), risk);
  }
});

test('AC-07.4: no rows renders an empty string', () => {
  assert.strictEqual(portal.renderOverdueRisk([]), '');
  assert.strictEqual(portal.renderOverdueRisk(), '');
  assert.strictEqual(portal.renderOverdueRisk(undefined), '');
  assert.strictEqual(portal.renderOverdueRisk(null), '');
});

test('AC-07.5: two-decimal amount strings are shown unchanged, beyond Number precision', () => {
  const big = '12345678901234567890.12';
  assert.notStrictEqual(Number(big).toFixed(2), big);
  for (const amount of ['1250.00', '15.00', big]) {
    assert.ok(portal.renderOverdueRisk([row({ amount })]).includes(`<td class="amount">${amount}</td>`), amount);
  }
});

test('AC-07.6: shorter amount strings are padded to two decimals as strings', () => {
  assert.ok(portal.renderOverdueRisk([row({ amount: '980.5' })]).includes('<td class="amount">980.50</td>'));
  assert.ok(portal.renderOverdueRisk([row({ amount: '980' })]).includes('<td class="amount">980.00</td>'));
});

test('AC-07.7: id and vendor text is HTML-escaped', () => {
  const html = portal.renderOverdueRisk([row({ id: 'SYN-INV-<1>', vendor: 'Synthetic "Vendor" & \'Co\' <b>' })]);
  assert.ok(html.includes('<td>SYN-INV-&lt;1&gt;</td>'));
  assert.ok(html.includes('<td>Synthetic &quot;Vendor&quot; &amp; &#39;Co&#39; &lt;b&gt;</td>'));
  assert.ok(!html.includes('<b>'));
});

test('AC-07.8: an unexpected risk value renders as risk-unknown', () => {
  for (const risk of ['critical', 'HIGH', '', undefined, null, '"><script>']) {
    const html = portal.renderOverdueRisk([row({ risk })]);
    assert.ok(html.includes('<tr class="risk-unknown">'), String(risk));
    assert.ok(!html.includes('<script>'));
  }
});

test('AC-07.9: no risk column, risk text or row action', () => {
  const html = portal.renderOverdueRisk([row({ risk: 'high' }), row({ id: 'SYN-INV-003', risk: 'low' })]);
  const text = html.replace(/<[^>]*>/g, ' ');
  assert.ok(!/risk|high|low/i.test(text));
  assert.strictEqual((html.match(/<th>/g) || []).length, 4);
  assert.ok(!/<(button|a|input|form)\b/i.test(html));
  assert.ok(!/\b(pay|post|approve|chase)\b/i.test(text));
});

test('AC-07.10: renderOverdueRisk is exported next to the unchanged exports; helpers stay private', () => {
  assert.deepStrictEqual(Object.keys(portal).sort(),
    ['LABELS', 'VERSION', 'formatAmount', 'renderAgingSummary', 'renderOverdueRisk']);
  assert.strictEqual(typeof portal.renderOverdueRisk, 'function');
});

test('rows that are not an array throw TypeError', () => {
  for (const rows of ['SYN-INV-001', 42, {}, { length: 1 }, true]) {
    assert.throws(() => portal.renderOverdueRisk(rows), ROWS_ERROR);
  }
});

test('days that are not an integer throw TypeError', () => {
  for (const days of [30.5, '31', undefined, null, NaN, Infinity]) {
    assert.throws(() => portal.renderOverdueRisk([row({ days })]), DAYS_ERROR);
  }
});

test('amounts that are not a decimal string with at most two decimals throw TypeError', () => {
  for (const amount of ['1e3', '1.25e3', '1,250.00', '1250.001', '', '.50', '12.', 'abc', ' 15.00', 1250, null, undefined]) {
    assert.throws(() => portal.renderOverdueRisk([row({ amount })]), AMOUNT_ERROR);
  }
});


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.