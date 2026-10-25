# Evaluate step FE-03: Identify the components/layers to be changed for FEAT-ZD-48600

- Workbook action: Read the existing code and understand it.
Identify the components/layers that needs to be created/updated with minimum lines of code
- Workbook outcome: The list of components/layers needs to be created/updated with minimum lines of code

## Outputs to evaluate
- `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md`

## Inputs the agent was given
- feat.tasks: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.tasks.md`
- feat.stories: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.stories.md`
- feat.prd: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.prd.md`
- feat.trd: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.trd.md`
- feat.design: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.design.md`
- task.repo_access: `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/task.repo_access.md`

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `~/.claude/projects/-Users-arghyamukherjee-Documents-automaiton-final-document-e2e-run-runs-run-20261025-205213-feat-zd-48600-workspace-ap-portal/74bf2ad2-44e0-438a-8813-bae8e4584869/tool-results/hook-53dded03-4d2b-4bab-9d01-17a0d9de25c7-1-additionalContext.txt`
- `hobasa-frontend-agent/.claude/skills/frontend-steps/SKILL.md`
- `hobasa-frontend-agent/CLAUDE.md`
- `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md`
- `ap-aging-sample/CLAUDE.md`
- `ap-forecast-sample/CLAUDE.md`
- `ap-pipeline-sample/CLAUDE.md`
- `ap-portal-sample/CLAUDE.md`
- `ap-portal-sample/README.md`
- `ap-portal-sample/ci.mjs`
- `ap-portal-sample/package.json`
- `ap-portal-sample/src/ap_portal/index.js`
- `ap-portal-sample/tests/portal.test.js`
- `hobasa-agentic-sdlc/CLAUDE.md`
- `hobasa-agentic-sdlc/features/FEAT-ZD-48600/JOURNEY.md`
- `hobasa-agentic-sdlc/memory/MEMORY.md`
- `final_document/hobasa-claude-code-plugin/skills/frontend-steps/SKILL.md`
- `final_document/hobasa-claude-code-plugin/skills/grounded-output/SKILL.md`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

# Step FE-03: Identify the components/layers to be changed (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Read the existing code and understand it.
Identify the components/layers that needs to be created/updated with minimum lines of code
- Workbook outcome: The list of components/layers needs to be created/updated with minimum lines of code
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- feat.tasks (Engineering tasks with goal, AC, dependencies and size budget): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.tasks.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md at commit b0fd4d2)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)
- feat.prd (Approved PRD): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.prd.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.prd.md at commit 908b58d)
- feat.trd (Approved TRD): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.trd.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.trd.md at commit 3637e51)
- feat.design (HLD, LLD and contracts): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)
- task.repo_access (Repository (and Databricks) availability and write access): `hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-03/task.repo_access.md` (from hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.repo_access.md at commit daae080)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.components (Components, layers, sources or pipelines to change): write `hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:npm (npm package metadata: versions, licence (GET /<package>)), mcp:codegraph (Code intelligence over the repository: a symbol's source, its callers and callees, the blast radius of a change and the tests it affects (codegraph_explore, codegraph_node and related tools; pass projectPath = the repository checkout)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

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