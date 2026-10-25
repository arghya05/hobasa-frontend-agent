# FE-03 · Identify the components/layers to be changed

**Agent:** Frontend · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Read the existing code and understand it.
Identify the components/layers that needs to be created/updated with minimum lines of code

**Expected outcome:** The list of components/layers needs to be created/updated with minimum lines of code

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `feat.stories` | Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision) | Requirement agent · REQ-08 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/feat.stories.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.stories.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.pdf) |
| 3 | `feat.prd` | Approved PRD | Requirement agent · REQ-05 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/feat.prd.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.prd.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-05_feat-prd.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-05_feat-prd.pdf) |
| 4 | `feat.trd` | Approved TRD | Requirement agent · REQ-07 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/feat.trd.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.trd.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-07_feat-trd.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-07_feat-trd.pdf) |
| 5 | `feat.design` | HLD, LLD and contracts | Design agent · DES-02 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/feat.design.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.design.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-02_feat-design.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-02_feat-design.pdf) |
| 6 | `task.repo_access` | Repository (and Databricks) availability and write access | Frontend agent · FE-01 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-03/task.repo_access.md) · [original](https://github.com/arghya05/hobasa-frontend-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-frontend-agent/main/docs/FEAT-ZD-48600/FE-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-frontend-agent/blob/main/docs/FEAT-ZD-48600/FE-01_task-repo_access.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.components` | Components, layers, sources or pipelines to change | [Markdown](https://github.com/arghya05/hobasa-frontend-agent/blob/main/outputs/FEAT-ZD-48600/task.components.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-frontend-agent/main/docs/FEAT-ZD-48600/FE-03_task-components.html) · [PDF](https://github.com/arghya05/hobasa-frontend-agent/blob/main/docs/FEAT-ZD-48600/FE-03_task-components.pdf) |

**Code:** repository [ap-portal-sample](https://github.com/arghya05/ap-portal-sample) · branch [feature/story](https://github.com/arghya05/ap-portal-sample/tree/feature/story) · commit [ff761ee](https://github.com/arghya05/ap-portal-sample/commit/ff761ee)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 5, 'complete': 5, 'grounded': 4, 'useful': 5} |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- CQ-FE-1 (a missing or non-string id/vendor renders as the text 'undefined') is marked non-blocking because feat.design B4.2 gives an interim answer (String(value)). That reading is defensible. Still, it makes the person at the gate confirm on purpose that rendering 'undefined' is acceptable, instead of the agent settling it in FE-04.
- The output says FC-8/FC-9 move VERSION 'one minor step from 2.3.0' and that the lane confirms the number. It could state the candidate value (2.4.0) as the proposal TK-07.5 asks for, marked as agent judgement and pending confirmation, so FE-04 has a concrete target.
- 'Minimum lines of code' is argued in words (one module, additive, no new file) but given no rough size. An approximate count of new lines per component, marked as agent judgement, would make the 'minimum' claim easier to check at the gate.
- Section 5 says 'The table template is split across lines where needed'. That is a coding instruction rather than a component; it could be labelled agent judgement like the other design choices.

## How the agent worked

- **Approach:** I used the FE-03 procedure from the frontend-steps skill. First I read the contracted inputs: feat.tasks, feat.stories, feat.design and task.repo_access in full, and the frontend-relevant parts of feat.prd and feat.trd (IF-7, F-12 to F-14, T-12 to T-16, AC-19 to AC-21, NFR-3/7/10). Then I read the whole ap_portal checkout with Read and Glob: index.js, portal.test.js, package.json, ci.mjs, README.md and CLAUDE.md. That gave me the existing layer, exports, callers and CI rules. I mapped the TSK-07 / feat.design B4 components onto the single existing view-helper module and picked the smallest additive change. Everything stays in index.js: one new export, private helpers, two constants, a version step and new tests, with no new file or layer. The verifier subagent had checked the earlier content against its sources. In this rewrite I took out the unsupported statement about how the code was read and kept only facts the sources support. I did not use codegraph, because the repository is one small module plus one test file and I read all of it directly.
- **Tools used:** Skill (frontend-steps), Read, Grep, Glob, Write, Agent (verifier)
- **Helper subagents:** Agent
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
