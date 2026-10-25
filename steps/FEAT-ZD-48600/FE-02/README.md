# FE-02 Access to secrets (Frontend agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check the access for required secrets and ask for access as needed  
**Outcome expected:** Secrets are available with access permissions

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-frontend-agent/inputs/FEAT-ZD-48600/FE-02/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.permission_table | not applicable or by reference | — | — | — |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.secrets_scope | hobasa-frontend-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md |

## Code

Repository `ap-portal-sample`, branch `feature/story`, commit `ff761ee`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the frontend-steps skill for FE-02 and got a receipt for my one readable contracted input, feat.tasks (revision b0fd4d2), through context_get. org.permission_table is restricted and passed by reference only, so I took the grant for ap_portal from the step's repo-access tool view, which summarises the permission table. From feat.tasks I worked out what the Frontend task TSK-07 needs: a pure HTML-rendering function in ap_portal, files in budget limited to index.js, package.json and the portal test file, no new technology, no transport layer and no LLM. So it needs no secret. I also looked through the ap_portal checkout for credential or environment-variable reads and found nothing beyond NODE_PATH in the CI script. I don't cite that check, so the claims rest only on receipted sources. The step blocked shell commands, so I used file tools only. Need and grant agree: none and none. So I raised no access request and wrote task.secrets_scope with names only.
- Tools used: Skill:frontend-steps, mcp__hobasa-context__context_list, mcp__hobasa-context__context_get (feat.tasks), mcp__hobasa-context__memory_search, Read, Grep, Glob, Write, Edit
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 5 says adding a secret or env-var read to `src/ap_portal/` 'would be outside the files in budget'. But `src/ap_portal/index.js` is in TSK-07's files in budget (feat.tasks TSK-07). The cited rule in feat.tasks section 2 covers only files outside the budget. Base the guardrail on scope or design instead, for example the no-transport rule and the pure rendering goal.
- Line 61 holds a stray line, '[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)', after the open questions. It looks like a pasted tool echo. Remove it or put it in the Sources line.
- The finding that TSK-07 needs no secret rests only on feat.tasks. The agent read the code, so it could cite `file:ap_portal/src/ap_portal/index.js` (no env or credential reads) and `file:ap_portal/ci.mjs` (its only `process.env` use passes NODE_PATH=src through to the tests). That would confirm the code needs no secret today.
- The 'dev (integration), main (released)' row says the task does not touch them. That is an inference from the feature-branch rule. A gate reader would find it clearer marked as such, or tied to the later deployment steps.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
