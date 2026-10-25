# Hobasa Frontend agent

Frontend Coding Agent in the Hobasa agentic SDLC. Owner: Frontend developer; UX lead for design.. It runs on Claude Code: every step is one Claude Code session started by the Hobasa spine, with this agent's prompt and step skill (`.claude/`), helper subagents, guardrail hooks and the shared context server.

**Reads from GitHub:** each step's inputs are prepared in `inputs/<trace>/<STEP>/` with the repository, path and commit they came from (other agents' `outputs/`, the requirements and organisation sources in `hobasa-agentic-sdlc`, code in the `*-sample` repositories).

**Writes back to GitHub:** approved outputs in `outputs/<trace>/`; every step run in `steps/<trace>/<STEP>/` (brief, attempts, claims, checks, independent judge, human decision). Code changes go to the `*-sample` repositories. Journey, evaluations and shared memory: `hobasa-agentic-sdlc`.

## Steps

| Step | Name | Reads | Writes | Gate |
|---|---|---|---|---|
| FE-01 | Checking the coding prerequisites ⁶ | feat.tasks, org.engineering_stack | task.repo_access | checks + sampled evaluation |
| FE-02 | Access to secrets | feat.tasks, org.permission_table | task.secrets_scope | checks + sampled evaluation |
| FE-03 | Identify the components/layers to be changed | feat.tasks, feat.stories, feat.prd, feat.trd, feat.design, task.repo_access | task.components | checks + sampled evaluation |
| FE-04 | Coding | task.components, feat.design, feat.stories, org.engineering_stack, org.lessons, task.secrets_scope, feat.screens, org.design_library | task.code | checks + sampled evaluation |
| FE-05 | Unit Test Cases | task.code, feat.stories | task.unit_tests | checks + sampled evaluation |
| FE-06 | Commit & Push | task.code, task.unit_tests, feat.stories | task.commit | checks + sampled evaluation |
| FE-07 | Code Quality Failure ³ ⁶ | task.commit | task.code, task.commit | checks + sampled evaluation |
| FE-08 | Share the PR for review ⁶ | task.commit | task.pr | PR reviewer |
| FE-09 | Update the defect ⁴ | feat.defects, task.pr | feat.defects | checks + sampled evaluation |
