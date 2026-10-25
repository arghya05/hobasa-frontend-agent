# Step FE-04: Coding (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Coding for the implementation required for changes in the components/layers with proper comments and required documentation
- Workbook outcome: Updated repo with the new implemented code changes
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.components (Components, layers, sources or pipelines to change): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/task.components.md` (from hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md at commit 76ba7e1)
- feat.design (HLD, LLD and contracts): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)
- org.engineering_stack (Repositories, languages, versions and exact commands): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.engineering_stack.md` (from hobasa-agentic-sdlc/org/org.engineering_stack.md at commit e8afce8)
- org.lessons (Approved lessons and eval cases): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.lessons.md` (from hobasa-agentic-sdlc/org/org.lessons.md at commit e8afce8)
- task.secrets_scope (Secrets granted for the task's environments (names only)): restricted, passed by reference only
- feat.screens (Approved final Figma screens): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.screens.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.screens.md at commit 722a98f)
- org.design_library (Hobasa design library and variables (tokens)): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.design_library.md` (from hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.design_library.md at commit e8afce8)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.code (Code changes on the working branch): write `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.code.md`

## Code
Work in repository `ap-portal-sample` (Node.js 22 (CommonJS, node:test)). You may change only: src/ap_portal/, docs/, README.md. Understand before you change: the repository is indexed with CodeGraph. Before editing, use it (MCP tools codegraph_explore and codegraph_node with projectPath `ap-portal-sample`, or `codegraph explore <symbols>`, `codegraph impact <symbol>`, `codegraph callers <symbol>`) to read the exact source of what you change, its callers and callees and the blast radius. After editing, run `codegraph sync` and `codegraph affected <changed files>` to find the tests your change touches, and run them through the CI. Shell commands allowed in this step: `node ci.mjs`, `git status`, `git diff`, `git log`, `codegraph explore`, `codegraph node`, `codegraph callers`, `codegraph callees`, `codegraph impact`, `codegraph affected`, `codegraph query`, `codegraph files`, `codegraph status`, `codegraph sync`. Run the CI yourself with `node ci.mjs` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does. Document what you change: update the repository's README.md and docs/ (module overview, new functions with signatures, inputs, outputs and examples).

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

module.exports = { VERSION, LABELS, formatAmount, renderAgingSummary };

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


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.