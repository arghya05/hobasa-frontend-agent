# FE-04 Coding (Frontend agent)

**Item:** FEAT-ZD-48600 · **status:** blocked
**Trigger:** Previous outcome  
**Action:** Coding for the implementation required for changes in the components/layers with proper comments and required documentation  
**Outcome expected:** Updated repo with the new implemented code changes

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| task.components | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/task.components.md | hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md | dbdee67 | FE-03 |
| feat.design | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.design.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md | 722a98f | DES-02 |
| feat.stories | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.stories.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md | 0590ea4 | REQ-08 |
| org.engineering_stack | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | a6b3e2a | source |
| org.lessons | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.lessons.md | hobasa-agentic-sdlc/org/org.lessons.md | a6b3e2a | source |
| task.secrets_scope | not applicable or by reference | — | — | — |
| feat.screens | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.screens.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.screens.md | 722a98f | DES-02 |
| org.design_library | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.design_library.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.design_library.md | a6b3e2a | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.code | — |

## Code

Repository `ap-portal-sample`, branch `feature/story`, commit `ff761ee`

```
no file changes in this step
```

## How the agent worked

- Approach: The refusal flagged the git diff line counts and the component tallies in the files-changed table. Neither appears in any source. I re-read the updated task.components input (revision dbdee67), whose content is unchanged, and re-ran `node ci.mjs` and git status to confirm the working tree.

I then rewrote task.code.md in full with these changes:
- The git diff statistics and the CI percentages and line ranges are gone; nothing here comes from a figure that no source holds.
- The files-changed table names each added constant and function from the tool view instead of counting them.
- The CI result is now stated qualitatively: compile passes, the two existing tests pass, there are no static issues, and coverage is below the 90% bar. The only numbers are those in task.components, org.engineering_stack and the tool view.

The code and docs in ap_portal are as shown in the tool view.
- Tools used: Read, Write, Bash (node ci.mjs, git status)
- Helper subagents: Agent
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 3 | — | — |
| 2 | resumed | 5 | — | — |
| 3 | resumed | 5 | — | — |
| 4 | resumed | 3 | 3 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 5 | resumed | 1 | 1 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 6 | resumed | 1 | 1 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Delete the stray line at the end of the file, "[Tool result] [ap_portal] Sources compile" (task.code.md:158). It is leftover tool output, not part of the document.
- Section 5 reports the CI run only in words: compile Pass, tests pass, static checks show no issues, coverage below 90%. Quote the actual JSON fields from the last line of `node ci.mjs` (compile, tests_run, failures, coverage, static_issues, passed) so the gate can check the result. No CI output is attached to the step, so the judge could not confirm these results.
- The brief says to run CI "until it passes", and CI still fails on coverage. CQ-1 explains the conflict (`tests/` is outside FE-04's write scope). It should be raised more visibly as a scope conflict for the spine owner, rather than given the interim answer 'Yes', which presumes the automated check will accept a failing CI.
- CQ-2 is real: `package.json` is still at 2.3.0 (file:ap_portal/package.json) while `VERSION` is now 2.4.0, and docs/ap_portal.md already says 'Package version: 2.4.0'. Make sure a later step owns FC-9, or ask for package.json to be added to the write scope.
- Optional: `escapeHtml(row.id)` turns a missing `id` or `vendor` into the text 'undefined' without raising an error. This differs from the strict TypeError handling for `days` and `amount` (DD-8). Note it as an open point or add it to the tests list.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
