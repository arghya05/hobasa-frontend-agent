# task.repo_access: FEAT-ZD-48600

Repository availability and write access for the Frontend lane of FEAT-ZD-48600 (ZD-48600, "Show which overdue invoices belong to vendors that usually pay late"). This is FE-01 Checking the coding prerequisites. The Frontend lane owns one task, TSK-07 (STORY-07, "Render the overdue risk table"), in the ap_portal repository (feat.tasks, sections 1 and 3).

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-01 Checking the coding prerequisites | record:brief |
| Trigger | Task creation, or any re-entry trigger | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (from DES-04, commit b0fd4d2), org.engineering_stack (commit 76d03ce) | record:brief |
| Task in scope | TSK-07 / STORY-07 / Frontend / ap_portal | feat.tasks (section 1) |

## 1. Result

**Outcome: the repository is available with write access. No repository has to be created, and no Databricks workspace or database is needed.**

| Check | Result | Source |
|---|---|---|
| Repository needed by the lane | ap_portal (the only Frontend repository; TSK-07) | feat.tasks (section 1, TSK-07) |
| Repository exists and is checked out | Yes, available locally | tool:repo-access; file:ap_portal/.git/HEAD |
| Repository creation needed | No: the repository already exists | tool:repo-access; org.engineering_stack |
| Working branch | `feature/story`, checked out (`HEAD` is `ref: refs/heads/feature/story`) | tool:repo-access; file:ap_portal/.git/HEAD |
| Branch base | Created from `dev` at commit `ff761ee`; the local `dev` and `main` branches point to the same commit | tool:repo-access; file:ap_portal/.git/logs/refs/heads/feature/story; file:ap_portal/.git/logs/refs/heads/dev; file:ap_portal/.git/logs/refs/heads/main |
| Branch model | `main` (released), `dev` (integration), `feature/<story>` | file:ap_portal/CLAUDE.md; org.engineering_stack; feat.tasks (section 2) |
| Working tree | Clean | tool:repo-access |
| Write access for the agent identity | Yes | tool:repo-access |
| Databricks / database / other workspace | Not used by this repository | tool:repo-access; org.engineering_stack |

## 2. Stack and CI binding for ap_portal

| Item | Value | Source |
|---|---|---|
| Language / runtime | Node.js 22, CommonJS (`'use strict'`, `module.exports`) | org.engineering_stack; file:ap_portal/CLAUDE.md; file:ap_portal/src/ap_portal/index.js |
| Test framework | `node:test` with `node:assert`; tests load the package as `require('ap_portal')` | org.engineering_stack; file:ap_portal/tests/portal.test.js |
| CI command | `node ci.mjs` (also `npm run ci` through the `ci` script in `package.json`) | org.engineering_stack; file:ap_portal/package.json |
| CI checks | `node --check` on every `src/ap_portal/*.js`; `node --test` with coverage of `src/**` and `NODE_PATH=src`; line coverage >= 90; at least one test run and no failures; no line matching `console.log` or `eval(`; lines at most 140 characters | file:ap_portal/ci.mjs |
| Code / tests location | `src/ap_portal/` / `tests/` | file:ap_portal/CLAUDE.md |
| Current version | `2.3.0` in `src/ap_portal/index.js` (`VERSION`) and in `package.json` (`"version"`) | file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json; feat.tasks (section 2) |
| Current exports | `VERSION`, `LABELS`, `formatAmount`, `renderAgingSummary` (kept unchanged) | file:ap_portal/src/ap_portal/index.js; feat.tasks (TSK-07 "Must not change") |
| Existing tests | `formats amounts with two decimals`, `renders every bucket in order` (kept unchanged) | file:ap_portal/tests/portal.test.js; feat.tasks (TSK-07 "Must not change") |
| Commit message rule | Starts with the story ID (`STORY-07`); the task ID `TSK-07` may follow | org.engineering_stack; feat.tasks (section 2, TA-5); record:memory |

## 3. Files TSK-07 will change (all already in the repository)

| File | Change allowed | Source |
|---|---|---|
| `src/ap_portal/index.js` | Constants, private helpers, the `renderOverdueRisk` export, `VERSION` step | feat.tasks (TSK-07 "Files in budget") |
| `package.json` | Version only | feat.tasks (TSK-07) |
| `tests/portal.test.js` | New tests; existing tests unchanged | feat.tasks (TSK-07) |

All three files exist in the checkout on `feature/story` (file:ap_portal/src/ap_portal/index.js; file:ap_portal/package.json; file:ap_portal/tests/portal.test.js). TSK-07 needs no new file or directory.

## 4. Dependencies affecting access

TSK-07 depends only on the CT-1 field list, not on TSK-06 code, so the Frontend lane does not need access to the ap_aging repository (feat.tasks, TSK-07 "Depends on"). This step needs no secret; FE-02 checks secrets.

## 5. Actions not taken (bounded by G9)

This step only reads. It did not push, merge, create a remote branch or run CI; the spine commits and pushes after evaluation. FE-01 does not allow shell commands, so the access facts come from the step's tool view (tool:repo-access) and the repository files read above.

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| RAQ-1 | feat.tasks and the repository's CLAUDE.md give the branch pattern `feature/<story>`, but the branch provided is `feature/story`, not `feature/STORY-07`. Should the lane work on `feature/story` or on a branch named for the story? | Work on `feature/story` as the spine provided; commit messages carry `STORY-07` for traceability | Developer |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Branch feature/story at ff761ee; write access confirmed
