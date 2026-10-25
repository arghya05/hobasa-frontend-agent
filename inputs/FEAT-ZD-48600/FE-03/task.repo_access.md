<!-- input task.repo_access for FE-03: from hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.repo_access.md at commit daae080, produced by FE-01 -->
# task.repo_access: FEAT-ZD-48600

Repository availability and write access for the Frontend lane of FEAT-ZD-48600. This is step FE-01, "Checking the coding prerequisites" (attempt 1). The Frontend lane owns one task: TSK-07 (STORY-07, "Render the overdue risk table"), in repository ap_portal (feat.tasks, sections 1 and 3).

Shell commands are not allowed in this step. Every repository fact below comes from the step's tool view (tool:repo-access) or from reading files in the checkout (file:ap_portal/...). Nothing was run, fetched or pushed.

## 1. Result

| Check | Result | Source |
|---|---|---|
| Repository needed by the Frontend lane | ap_portal (TSK-07 is the only Frontend task) | feat.tasks (section 1, TSK-07) |
| Repository available | Yes. It is checked out locally | tool:repo-access; file:ap_portal/.git/HEAD |
| Repository creation needed | No. ap_portal already exists | tool:repo-access; org.engineering_stack |
| Write access | Yes. The agent identity has write access and `git status` is clean | tool:repo-access |
| Working branch | `feature/story`, created from `dev` at `ff761ee`. HEAD is on this branch | tool:repo-access; file:ap_portal/.git/HEAD; file:ap_portal/.git/logs/HEAD |
| Base branches | `main` (released) and `dev` (integration) both point to `ff761ee` | file:ap_portal/.git/refs/heads/main; file:ap_portal/.git/refs/heads/dev; file:ap_portal/CLAUDE.md |
| Remote | `origin` on GitHub (repository `ap-portal-sample`). `main` and `dev` track it. `feature/story` has no upstream yet. The spine pushes it at FE-06 | file:ap_portal/.git/config |
| Databricks or other workspace | Not used | tool:repo-access; org.engineering_stack |
| Database | Not used | org.engineering_stack |
| Outcome | **Met.** The repository is available and has write access | tool:repo-access |

## 2. Repository profile (for FE-03 and FE-04)

| Item | Value | Source |
|---|---|---|
| Language / runtime | Node.js 22, CommonJS, tests with `node:test` | org.engineering_stack; file:ap_portal/CLAUDE.md |
| CI command | `node ci.mjs` | org.engineering_stack; file:ap_portal/package.json |
| CI checks | Each `src/ap_portal/*.js` file passes `node --check`. Tests run with `NODE_PATH=src`. Line coverage must be at least 90%. No line may match `console.log` or `eval(`. No line may be longer than 140 characters | file:ap_portal/ci.mjs; feat.tasks (section 2, CI bar) |
| Source | `src/ap_portal/index.js` exports `VERSION`, `LABELS`, `formatAmount` and `renderAgingSummary` | file:ap_portal/src/ap_portal/index.js |
| Tests | `tests/portal.test.js` holds the two existing tests: amount formatting and bucket rendering | file:ap_portal/tests/portal.test.js |
| Current version | `2.3.0`, in both `src/ap_portal/index.js` and `package.json` | file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json; feat.tasks (section 2) |
| TSK-07 files in budget | `src/ap_portal/index.js`, `package.json` (version only), `tests/portal.test.js`. All three exist on the working branch | feat.tasks (TSK-07); file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json; file:ap_portal/tests/portal.test.js |

## 3. Branch and commit rules for this lane

- Branch convention: `feature/<story>`. The repository also has `main` (released) and `dev` (integration) (feat.tasks, section 2; file:ap_portal/CLAUDE.md).
- Commit messages start with the story ID (`STORY-07`). The task ID (`TSK-07`) may follow (feat.tasks, section 2; record:memory conventions).
- This agent may branch, commit and open a PR. It does not merge, deploy or push to protected `main` (agent guardrails, record:brief).

## 4. Open questions (non-blocking)

| ID | Question | Owner |
|---|---|---|
| RA-1 | The working branch is named `feature/story`, but the convention is `feature/<story>`. For TSK-07 that would be `feature/STORY-07`. Keep `feature/story`, or have the spine rename it before FE-06? | Developer |
| RA-2 | Write access to the GitHub remote has not been tested, because this step allows no network or push. The tool view confirms local write access only. The spine's push at FE-06 is the first real test of remote write access | Developer / spine |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Branch feature/story at ff761ee; write access confirmed
