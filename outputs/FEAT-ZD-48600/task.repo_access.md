# task.repo_access: FEAT-ZD-48600 (Frontend lane)

This file records the coding prerequisites for the Frontend task of FEAT-ZD-48600. It answers three questions: is the repository available, does it need to be created, and can the agent identity write to it. Step: FE-01, Checking the coding prerequisites.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-01 Checking the coding prerequisites | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Task in scope | TSK-07 (STORY-07): render the overdue risk table SC-1 with `renderOverdueRisk` | feat.tasks (sections 1 and 3, TSK-07) |
| Inputs | feat.tasks (design-agent output at b0fd4d2); org.engineering_stack (at 3b03f15) | record:brief |

## 1. Result

**Outcome: the repository is available and the agent identity can write to it.** No repository needs to be created. No Databricks workspace or database is needed.

| Check | Result | Evidence |
|---|---|---|
| Repository named by the task | `ap_portal` (Frontend lane) | feat.tasks (section 1, TSK-07) |
| Repository available locally | Yes | tool:repo-access; file:ap_portal/.git/HEAD |
| Repository creation needed | No. It already exists and has history at commit `ff761ee` | tool:repo-access; file:ap_portal/.git/refs/heads/dev |
| Long-lived branches | `main` (released) and `dev` (integration) both exist and both point at `ff761ee` | file:ap_portal/.git/refs/heads/main; file:ap_portal/.git/refs/heads/dev; file:ap_portal/CLAUDE.md |
| Working branch | `feature/story`, created from `dev` at `ff761ee`; it is checked out | tool:repo-access; file:ap_portal/.git/HEAD; file:ap_portal/.git/logs/refs/heads/feature/story |
| Write access for the agent identity | Yes. The working tree is clean | tool:repo-access |
| Databricks or other workspace | Not used by this repository | tool:repo-access; org.engineering_stack |
| Database | None used | org.engineering_stack |

## 2. Stack and CI, as found in the repository

| Item | Value | Source |
|---|---|---|
| Language / test runner | Node.js 22, CommonJS, `node:test` | org.engineering_stack; file:ap_portal/CLAUDE.md |
| CI command | `node ci.mjs` | org.engineering_stack; file:ap_portal/CLAUDE.md; file:ap_portal/ci.mjs |
| CI checks | Runs `node --check` on every `src/ap_portal/*.js` file, then the tests with `--experimental-test-coverage` and `NODE_PATH=src`. CI passes only if coverage is >= 90, no test fails, at least one test runs and there are no static issues | file:ap_portal/ci.mjs |
| Static checks (stand-in for SonarQube) | No line matching `console.log` or `eval(`; no line longer than 140 characters | file:ap_portal/ci.mjs; feat.tasks (section 2, CI bar) |
| Code and test folders | Code in `src/ap_portal/`, tests in `tests/`; the files are `src/ap_portal/index.js` and `tests/portal.test.js` | file:ap_portal/CLAUDE.md; file:ap_portal/src/ap_portal/index.js; file:ap_portal/tests/portal.test.js |
| Current version | `2.3.0`, in both `src/ap_portal/index.js` (`VERSION`) and `package.json` | file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json; feat.tasks (section 2) |
| Current exports | `VERSION`, `LABELS`, `formatAmount`, `renderAgingSummary` | file:ap_portal/src/ap_portal/index.js |
| Existing tests | `formats amounts with two decimals`; `renders every bucket in order` | file:ap_portal/tests/portal.test.js |

## 3. Files and rules for the Frontend lane (from the task budget)

| Item | Value | Source |
|---|---|---|
| Files in budget | `src/ap_portal/index.js`, `package.json` (version only), `tests/portal.test.js` | feat.tasks (TSK-07) |
| Must not change | `LABELS`, `formatAmount`, `renderAgingSummary` and their tests | feat.tasks (TSK-07) |
| Signature rule | Keep the existing public signatures and add new functions for new behaviour | file:ap_portal/CLAUDE.md; feat.tasks (section 2) |
| Commit messages | Start with the story ID (`STORY-07`); the task ID may follow | feat.tasks (section 2); org.engineering_stack; record:memory |
| Version step | Owned by TSK-07. `VERSION` and `"version"` change together, and the lane confirms the next number | feat.tasks (section 2, TSK-07) |

All three files in the budget exist in the repository (file:ap_portal/package.json; file:ap_portal/src/ap_portal/index.js; file:ap_portal/tests/portal.test.js).

## 4. Notes and open questions (non-blocking)

| ID | Note or question | Interim answer |
|---|---|---|
| RA-1 | The branch convention is `feature/<story>` (file:ap_portal/CLAUDE.md; feat.tasks section 2), but the working branch is named `feature/story`, not `feature/STORY-07` (tool:repo-access). Should it be renamed? | Use `feature/story` and start every commit message with the story ID. The Developer confirms whether to rename it |
| RA-2 | Shell commands are not allowed in FE-01, so `git status` and CI were not run in this step. Write access and the clean tree come from the step's tool view, and the branch refs were read from the `.git` files | Accept the tool view. CI first runs in the coding steps |
| RA-3 | `dev` and `main` both point at commit `ff761ee` | No action needed. This records the starting point for later rebases |


[Tool result] [ap_portal] Branch feature/story at ff761ee; write access confirmed