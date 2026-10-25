# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Frontend task TSK-07 (STORY-07, "Render the overdue risk table") in repository `ap_portal`. This is FE-02 Access to secrets (attempt 1). It lists secret names only. No secret values appear here, and none were read.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | FE-02 Access to secrets (attempt 1) | record:brief |
| Task in scope | TSK-07 / STORY-07, Frontend lane, `ap_portal` | feat.tasks (sections 1, 3 TSK-07) |
| Inputs | feat.tasks (from hobasa-design-agent at commit b0fd4d2); org.permission_table (restricted, passed by reference only) | record:brief |
| Permission source | Tool registry for `ap_portal` | tool:repo-access |

## 1. Secrets required by the task

**None.** TSK-07 adds a pure render function, `renderOverdueRisk(rows)`, plus private helpers and constants in `src/ap_portal/index.js`, a version step in `package.json`, and tests in `tests/portal.test.js` (feat.tasks TSK-07). It takes the CT-1 rows as an argument. It has no transport layer, network call, API key, database or LLM access. The task depends only on the CT-1 field list, not on Backend code (feat.tasks TSK-06, TSK-07).

What the repository shows:

| Check | Finding | Source |
|---|---|---|
| Runtime dependencies | `package.json` declares no dependencies, only the `ci` script `node ci.mjs` | file:ap_portal/package.json |
| Environment use in CI | `ci.mjs` copies `process.env` and sets only `NODE_PATH: 'src'`; it reads no secret variable | file:ap_portal/ci.mjs |
| Secret references in the repository | A case-insensitive search for `process.env`, `secret`, `token`, `apikey`, `api_key` and `password` (excluding `.git`) found one match: the `NODE_PATH` line in `ci.mjs` | file:ap_portal/ci.mjs |
| Technology | Node.js 22 CommonJS and `node:test` only; no new technology | feat.tasks (header, DD-1); file:ap_portal/CLAUDE.md |

## 2. Secrets granted for the task's environments (names only)

| Environment | Secrets required | Secrets granted | Access request needed |
|---|---|---|---|
| Local / CI (`node ci.mjs`) | None | None | No |
| `feature/<story>` → `dev` (integration) | None | None | No |
| `main` (released) | None | None | No |

The permission table grants no secrets to this repository, and none are required (tool:repo-access). Required and granted match, so the task does not need extra access and has no excess access to flag.

## 3. Access requests raised

None. No `capability_request` was needed.

## 4. Notes for later steps

- FE-04 Coding should not add environment variables, API keys or network calls. TSK-07 is a pure render function (feat.tasks TSK-07). If the scope changes and a secret becomes necessary, re-run this step; it is a re-entry trigger.
- The end-to-end harness TSK-08 belongs to the Testing Agent and runs on a synthetic feed only (feat.tasks TSK-08). It is outside this Frontend secrets scope.

## 5. Open questions

None blocking. None non-blocking.

## 6. Validation check

| Check | Result | Source |
|---|---|---|
| Secret names only, no values | Pass | record:brief |
| Required secrets match granted secrets | Pass (none required, none granted) | tool:repo-access; feat.tasks (TSK-07) |
| Restricted permission table passed by reference only | Pass (not quoted) | record:brief |
| Blocking questions | None | — |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_portal] Tool registry: no secrets required or granted (permission table)
