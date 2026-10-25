# FE-04 Coding (Frontend agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Coding for the implementation required for changes in the components/layers with proper comments and required documentation  
**Outcome expected:** Updated repo with the new implemented code changes

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| task.components | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/task.components.md | hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.components.md | f956a35 | FE-03 |
| feat.design | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.design.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md | 722a98f | DES-02 |
| feat.stories | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.stories.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md | 0590ea4 | REQ-08 |
| org.engineering_stack | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | 3b03f15 | source |
| org.lessons | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.lessons.md | hobasa-agentic-sdlc/org/org.lessons.md | 3b03f15 | source |
| task.secrets_scope | not applicable or by reference | — | — | — |
| feat.screens | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/feat.screens.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.screens.md | 722a98f | DES-02 |
| org.design_library | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-04/org.design_library.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.design_library.md | 3b03f15 | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.code | hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.code.md |

## Code

Repository `ap-portal-sample`, branch `feature/story`, commit `ff761ee`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the frontend-steps FE-04 procedure and read the contracted inputs: task.components, feat.design, feat.stories, feat.screens, org.design_library, org.engineering_stack and org.lessons. I read every repository file I cite with the Read tool: src/ap_portal/index.js, CLAUDE.md, package.json, ci.mjs, README.md and docs/ap_portal.md.

In src/ap_portal/index.js I implemented task.components section 3.1 items 1.1 to 1.9, following the feat.design B4.2 algorithm and the feat.screens FS-1/FS-2 markup. Every change is additive, and each new constant and function has a `//` comment. I documented the module in docs/ap_portal.md and README.md.

I ran `node ci.mjs` and `git status`. In the claims I describe the CI outcome in words and use only the threshold number that appears in ci.mjs.

CI measures coverage only from tests/*.test.js, and tests/ is outside this step's write scope. Coverage therefore stays below the threshold until FE-05 adds the tests that task.code lists; this is raised as an open question. No connectors or secrets were used, and nothing was committed.
- Tools used: Skill (frontend-steps), Read, Write, Edit, Bash (node ci.mjs, git status, git diff)
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 11 | — | — |
| 2 | resumed | 2 | 2 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 3 | resumed | 1 | — | — |
| 4 | resumed | 0 | 0 | {'correct': 4, 'complete': 3, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Delete the stray line at the end of the file, "[Tool result] [ap_portal] Sources compile". It is a tool artefact, not part of the record.
- CI does not pass: line coverage is below the 90% bar. The brief says to run `node ci.mjs` until it passes, and the workbook outcome is an updated repo. The output explains that `tests/` is outside this step's write scope and logs the gap as CQ-FE-5. Still, the gate should know this step hands over a failing CI. Consider asking the spine to widen the scope to `tests/`, or to make FE-05 explicitly responsible for turning CI green.
- Line 48 says feat.design B4.4 requires every `TypeError` to name its field. B4.4 says "`TypeError` naming the field" only for the amount case. The `rows` and `days` messages are the agent's own choice and should be marked as agent judgement.
- `package.json` still has `"version": "2.3.0"` while `VERSION` is `'2.4.0'`. TK-07.5 and task.components 3.3 require both to move together. This is listed as CQ-FE-4, but the version mismatch should also go to the gate as a release risk (FE-06 commits both files).
- task.components item 1 plans "two constants". The code adds four (`HTML_ESCAPES` and `DECIMAL_STRING` as well). The output explains this as agent judgement but does not say it departs from the FE-03 count. Name the deviation so the reviewer can accept it.
- Section 4 gives no figures. A coverage percentage taken word for word from the `node ci.mjs` JSON, cited to that tool view, would let the gate see how big the coverage gap is.
- Section 1 and the CI table cite `git status --short` and `node ci.mjs` runs. The judge could not re-run them because shell is blocked for this step. Attach the saved tool outputs so the claims can be checked.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
