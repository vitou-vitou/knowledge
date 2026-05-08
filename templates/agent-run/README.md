# Agent Run Template

> Drop this folder into a project as `docs/agents/`. Fill in the 5 role specs from
> `D:\vitou\knowledge\playbooks\*-template.md`, then launch 5 parallel agents.

## Steps

1. Copy this folder to `<your-project>/docs/agents/`.
2. For each role you need (you don't have to use all 5):
   - Copy the matching `D:\vitou\knowledge\playbooks\<role>-template.md` into
     `<your-project>/docs/agents/0X-<role>-<topic>.md`.
   - Replace every `[PLACEHOLDER]`.
3. Verify scopes do not overlap. Architect/QA usually only write new docs/tests.
4. Launch all chosen agents in **a single message** with parallel `Task` calls,
   `run_in_background: true`. Use the model slug from each playbook header.
5. Wait for completion notifications. Review handoff docs in `docs/agents/handoff/`.

## Default 5-Role Slate

| # | Role | Model | Source playbook |
|---|---|---|---|
| 01 | Bugfix | `gpt-5.3-codex-high-fast` | `bugfix-template.md` |
| 02 | Feature | `claude-4.6-sonnet-medium-thinking` | `feature-template.md` |
| 03 | DevOps | `gpt-5.5-medium` | `devops-template.md` |
| 04 | QA | `composer-2-fast` | `qa-template.md` |
| 05 | Architect (read-only) | `claude-opus-4-7-thinking-xhigh` | `architect-template.md` |

## Conflict Avoidance Rules (apply to every agent)

1. **Stay within your scope.** If you need to change a file outside your scope, write a
   note in `docs/agents/handoff/<your-id>-handoff.md` instead of editing.
2. **Do not touch** `composer.json`, `composer.lock`, `package.json`, `package-lock.json`
   unless your task explicitly requires it.
3. **Do not run** `composer install`, `npm install`, `php artisan migrate`, or
   `docker build`.
4. **Commit nothing.** Leave changes in the working tree for the user to review.
5. **Preserve existing typos in identifiers** (e.g. legacy file names) unless the user
   confirms a rename.

## Reference

- Strategy: `D:\vitou\knowledge\strategies\SAAS-MULTI-AGENT-STRATEGY.md`
- Coordination: `D:\vitou\knowledge\strategies\PARALLEL-AGENTS-ADVANCED.md`
- Models: `D:\vitou\knowledge\strategies\MODELS-RANKING.md`
