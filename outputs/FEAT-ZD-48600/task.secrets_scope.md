# task.secrets_scope: FEAT-ZD-48600 (Frontend, ap_portal)

Secrets scope for the Frontend task TSK-07 "Render the overdue risk table" (STORY-07). This is FE-02 Access to secrets (attempt 1). The step checks which secrets the task needs, which ones the permission table grants, and whether access must be requested.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-02 Access to secrets (attempt 1) | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (revision b0fd4d2, from DES-04); org.permission_table (restricted, passed by reference only) | record:brief; feat.tasks |
| Task in scope | TSK-07 / STORY-07, Frontend lane, repository ap_portal | feat.tasks (sections 1, 3 TSK-07) |

## 1. Result

**No secrets are required, and none are granted, for TSK-07 in ap_portal. No access request is needed.**

| Secret name | Environment | Needed by TSK-07 | Granted (permission table) | Action |
|---|---|---|---|---|
| None | None | No | No: "no secrets are required or granted for this repository" | None. No access request raised |

Sources: tool:repo-access (permission table summary for ap_portal); feat.tasks (TSK-07).

## 2. Why the task needs no secret

TSK-07 is a pure rendering change inside the ap_portal package. Nothing in its scope reads a credential, calls a service or opens a connection.

| Check | Finding | Source |
|---|---|---|
| What the task builds | Exported `renderOverdueRisk(rows)` that returns an HTML `<table class="overdue-risk">` string, or `''` for missing, `null` or `[]` | feat.tasks (TSK-07 Goal) |
| Files in budget | `src/ap_portal/index.js`, `package.json` (version only), `tests/portal.test.js` | feat.tasks (TSK-07 Files in budget) |
| Technology | No new technology; only the Node.js 22 CommonJS runtime for this lane | feat.tasks (preamble, DD-1 carried) |
| Data source | Takes the CT-1 rows as an argument; it depends on the CT-1 field list only, not on TSK-06 code | feat.tasks (TSK-07 Depends on) |
| Transport / network | None. CT-1 has no transport layer; adding one would re-trigger DES-01 | feat.tasks (TSK-06 Must not change) |
| External services or LLM | None in the Frontend lane. The only LLM-related task is TSK-04 in ap_forecast, which is not part of this lane | feat.tasks (sections 1, 3 TSK-04) |
| Tests and CI | `node:test` with `NODE_PATH=src`; CI bar is compile, unit tests, coverage of at least 90% and static checks | feat.tasks (TSK-07 Tests to add; section 2 CI bar) |
| End-to-end proof | TSK-08 uses a synthetic feed only (`SYN-INV-*`) and is the Testing Agent's test asset, not deployed | feat.tasks (TSK-08) |

## 3. Environments

| Environment | Use by TSK-07 | Secrets |
|---|---|---|
| Local / CI (feature branch `feature/<story>`) | Compile, unit tests, coverage and static checks | None |
| dev (integration), main (released) | Not touched by this task, which works on its feature branch | None granted for ap_portal |

Sources: feat.tasks (section 2 Branch and commit, CI bar); tool:repo-access.

## 4. Access requests

None raised. The permission table grants nothing for ap_portal and the task needs nothing, so the scope matches and there is no excess access to flag.

## 5. Guardrails carried to FE-04 Coding

- Do not add any secret, token, key or environment-variable read to `src/ap_portal/` for TSK-07. If one ever looks necessary, stop and return to FE-02. It would be outside the files in budget and outside the design (feat.tasks section 2: a task that needs a file outside its budget goes back to the Architect).
- Test data stays synthetic (feat.tasks TSK-08).

## 6. Open questions

None blocking. None new.


[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)