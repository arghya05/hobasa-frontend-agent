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
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- The workbook asks for the change 'with minimum lines of code', but the output gives no size for the change. A rough line estimate per component (C-FE-1 to C-FE-10) would make the claim checkable at FE-04.
- In 'How this keeps the change small' item 1, 'ci.mjs would also have to compile it' makes this sound like extra work. ci.mjs already runs `node --check` on every .js file in src/ap_portal automatically, so a new file needs no CI change. The real reason to keep one file is feat.design DD-2, which puts renderOverdueRisk in index.js. Say that instead.
- AC-07.9 (no Risk column or risk text, no row actions) and NFR-10 (no 'Late' wording) have no test in the section 4 coverage table. Consider an assertion in `renders overdue risk table` that the output contains no 'Risk' or 'Late' heading.
- The B4.5 escaping test covers only `<`, `&` and `"`, but AC-07.7 also requires `>` and `'`. Consider adding them so every entry in the escape map is checked.
- In the section 5 traceability table, the C-FE-2/C-FE-6 row links AC-07.9 to T-12 and T-13. No TRD test covers AC-07.9 (feat.stories maps NFR-6 / AC-07.9 to '—'). Split that row so the gap is visible.

## How the agent worked

- **Approach:** I used the frontend-steps FE-03 procedure. First I read feat.tasks (TSK-07), feat.design (B4 LLD, CT-1, DD-5, DD-8) and task.repo_access in full. From feat.stories, feat.prd and feat.trd I searched only the STORY-07, AC-19 to AC-21 and IF-7 / T-12 to T-16 parts. Then I read the existing ap_portal code: the index.js and test views, ci.mjs, package.json and README. I asked codegraph which code depends on the existing exports. From that I listed the smallest set of changes: two constants, three private helpers and one exported function, all appended in index.js. Two existing lines change (VERSION and module.exports), plus the package.json version and new tests. There are no new files and no edits to existing logic. A verifier subagent checked the draft against its sources. I fixed one citation and one overreach it found, and marked the codegraph test-coverage remark as agent judgement. A shell check was blocked by the step's G9 guardrail, so I read the files with Read and Glob instead.
- **Tools used:** Skill(frontend-steps), Read, Grep, Glob, mcp:codegraph (codegraph_explore), Agent(verifier), Write, Edit
- **Helper subagents:** Agent
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
