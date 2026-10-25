# FE-02 · Access to secrets

**Agent:** Frontend · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Check the access for required secrets and ask for access as needed

**Expected outcome:** Secrets are available with access permissions

**Started by:** Task creation, or any re-entry trigger ¹

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-frontend-agent/blob/main/inputs/FEAT-ZD-48600/FE-02/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `org.permission_table` | Allowed tools, secrets and data per agent | — | not applicable for this item, or passed by reference |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.secrets_scope` | Secrets granted for the task's environments (names only) | [Markdown](https://github.com/arghya05/hobasa-frontend-agent/blob/main/outputs/FEAT-ZD-48600/task.secrets_scope.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-frontend-agent/main/docs/FEAT-ZD-48600/FE-02_task-secrets_scope.html) · [PDF](https://github.com/arghya05/hobasa-frontend-agent/blob/main/docs/FEAT-ZD-48600/FE-02_task-secrets_scope.pdf) |

**Code:** repository [ap-portal-sample](https://github.com/arghya05/ap-portal-sample) · branch [feature/story](https://github.com/arghya05/ap-portal-sample/tree/feature/story) · commit [ff761ee](https://github.com/arghya05/ap-portal-sample/commit/ff761ee)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- Section 4 says FE-02 'must run again, because that would be a re-entry trigger' if a network call, registry-token dependency or deployed environment is added. No source defines the re-entry triggers (footnote ¹ in the workbook is not expanded anywhere the agent read). Label this as agent judgement, or point it to whoever owns the trigger list.
- Section 4 does not say who an access request would go to, or through which channel (for example capability_request to the owner of org.permission_table), if a secret is needed later. Naming both would make the 'ask for access as needed' part of the action easier to act on.
- The CI row says ci.mjs 'reads no named secret', which is true. It could also note that ci.mjs passes the whole of process.env to the test process. Any secret present in the CI host environment would therefore reach the tests, even though none is granted or read.
- The environments 'local build, CI' in section 2 come from inference (ci.mjs and the CI bar in feat.tasks). Cite file:ap_portal/ci.mjs and feat.tasks section 2 (CI bar) next to tool:repo-access for that column.

## How the agent worked

- **Approach:** I followed the FE-02 procedure in the frontend-steps skill. First I read feat.tasks to find the Frontend task: TSK-07 in ap_portal, which renders HTML from given rows, uses no transport layer and touches only the files in its budget. The permission table is restricted, so I used the tool view's summary of it. Shell commands are blocked for FE-02, so I checked the ap_portal checkout with Glob, Grep and Read. I read src/ap_portal/index.js, package.json, ci.mjs and tests/portal.test.js in full to look for secret, token or environment use, dependencies and network calls. The task needs no secret and the permission table grants none, so there is no gap and I raised no access request. I wrote task.secrets_scope with secret names only, which here means none.
- **Tools used:** Skill (frontend-steps), Read, Glob, Grep, Write
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
