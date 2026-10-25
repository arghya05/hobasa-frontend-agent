# FE-05 · Unit Test Cases

**Agent:** Frontend · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage ²

**Expected outcome:** Unit testing completed

**Started by:** Code completion

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `task.code` | Code changes on the working branch | Frontend agent · FE-04 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-05/task.code.md) · [original](https://github.com/arghya05/hobasa-frontend-agent/blob/main/outputs/FEAT-ZD-48600/task.code.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-frontend-agent/main/docs/FEAT-ZD-48600/FE-04_task-code.html) · [PDF](https://github.com/arghya05/hobasa-frontend-agent/blob/main/docs/FEAT-ZD-48600/FE-04_task-code.pdf) |
| 2 | `feat.stories` | Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision) | Requirement agent · REQ-08 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-05/feat.stories.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.stories.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.unit_tests` | Unit tests and coverage (≥90%) | [Markdown](https://github.com/arghya05/hobasa-frontend-agent/blob/main/outputs/FEAT-ZD-48600/task.unit_tests.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-frontend-agent/main/docs/FEAT-ZD-48600/FE-05_task-unit_tests.html) · [PDF](https://github.com/arghya05/hobasa-frontend-agent/blob/main/docs/FEAT-ZD-48600/FE-05_task-unit_tests.pdf) |

**Code:** repository [ap-portal-sample](https://github.com/arghya05/ap-portal-sample) · branch [feature/story](https://github.com/arghya05/ap-portal-sample/tree/feature/story) · commit [ff761ee](https://github.com/arghya05/ap-portal-sample/commit/ff761ee)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 2 | — | — |
| 2 | continued | 2 | 2 | {'correct': 4, 'complete': 5, 'grounded': 4, 'useful': 5} |
| 3 | continued | 0 | 0 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- Test 1 row (line 23) cites tool:repo, but the tool view in the brief is a snapshot from before the edit and shows the opposite input order (SYN-INV-002/42 days first). The current file:ap_portal/tests/portal.test.js (lines 26-34) has SYN-INV-001 (31 days) first, which matches the output. Cite file:ap_portal/tests/portal.test.js for this claim so the citation supports it.
- Line 19 says the test vendors are named "Synthetic Vendor <letter>". The AC-07.7 escaping test uses the vendor 'Synthetic "Vendor" & 'Co' <b>' and the id 'SYN-INV-<1>'. Say that the escaping test deliberately breaks this pattern. 'Each test overrides only the field it checks' also does not hold for tests 1 and 9, which override several fields.
- Line 11 says `node ci.mjs` exits 0 and `passed` is true. The only CI result recorded by the spine is '15 run, 0 failing; coverage 100%', so compile and static_issues come only from the agent's own run. Quote the JSON line from the CI run (compile, tests_run, failures, coverage, static_issues, passed), as task.code section 1.1 did, so the claim can be checked against a recorded value.
- Section 1 could state the test count, 15, from the spine tool result: the 2 existing tests plus the 13 new ones in section 2. That ties the table directly to the recorded CI result.
- Line 11 says `passed` requires compile, no failing test, coverage >= 90 and no static issues. It leaves out two other conditions in ci.mjs line 17: r.status === 0 and tests_run > 0. List all the conditions.

## How the agent worked

- **Approach:** I followed the FE-05 procedure in the frontend-steps skill and read task.code and feat.stories. I read the exact source and blast radius of renderOverdueRisk and its helpers with the codegraph MCP tool (codegraph_explore). I added node:test cases to tests/portal.test.js after the existing tests, which I did not change. There is one test for each of AC-07.1 to AC-07.10, plus the TypeError coverage cases that task.code section 6 hands over. All test data is synthetic SYN-INV data. In the AC-07.1 test, the input rows have fewer days first, the reverse of the ap_aging sort, so a re-sort inside renderOverdueRisk would fail it. I ran `node ci.mjs` from the repository root and it exited 0 (passed). I then ran `codegraph sync` and `codegraph affected`, and the only affected test file is tests/portal.test.js. Last, I wrote task.unit_tests with a table that traces each test to its acceptance criterion.
- **Tools used:** Skill:frontend-steps, Read, Edit, Write, mcp:codegraph (codegraph_explore), Bash:node ci.mjs, Bash:codegraph sync, Bash:codegraph affected, Bash:git status
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
