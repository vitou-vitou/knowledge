# PGI Knowledge Base — Global Cursor Reference

> Single source of truth for cross-project methods, stacks, and playbooks.
> Local: `D:\vitou\knowledge\` | Remote: <https://github.com/vitou-vitou/knowledge>
> Owner: Vitou | Created: 2026-05-08

Any Cursor agent working on a project under `D:\vitou\projects\` should treat
this folder as the authoritative reference for **how we work**. Project-specific
context still lives inside each project's own `docs/`.

---

## Contents

| Folder | What lives here | When to read |
|---|---|---|
| `strategies/` | Cross-project methods (multi-agent runs, model selection, chat continuation) | Planning a new build or a parallel run |
| `stacks/` | Tech-specific knowledge (Laravel, future: React/Vue/Next/Python) | Working in that stack |
| `playbooks/` | Reusable role prompts (bugfix / feature / devops / qa / architect) | Composing a parallel agent run |
| `templates/` | Forkable scaffolds (e.g. `agent-run/` you can copy into a new project's `docs/agents/`) | Starting a new run |
| `ideas/` | SaaS / product ideas, market research | Picking the next thing to build |
| `ops/` | Disk hygiene, trusted resources, dev-machine ops | One-off ops work |

---

## File Index

### `strategies/`

- **`SAAS-MULTI-AGENT-STRATEGY.md`** — The "5 specialist agents in parallel" core method.
- **`PARALLEL-AGENTS-ADVANCED.md`** — Wave execution, hub-and-spoke, conflict resolution hierarchy.
- **`MODELS-RANKING.md`** — Which Cursor model for which job.
- **`HOW-TO-CONTINUE-CHATS.md`** — Resuming long conversations without losing context.

### `stacks/laravel/`

- **`LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`** — Stack notes for Laravel + Filament + Livewire builds.
- **`LARAVEL-QUICK-LAUNCH.md`** — Cold-start a Laravel project fast.

### `playbooks/`

Role-prompt templates with `[PROJECT]`, `[SCOPE]`, `[FILES]` placeholders. Fork into
`<project>/docs/agents/0X-<role>.md` and fill in.

- `bugfix-template.md`
- `feature-template.md`
- `devops-template.md`
- `qa-template.md`
- `architect-template.md`

### `templates/agent-run/`

- `README.md` — Drop this folder into any project as `docs/agents/`, fill in the 5 role specs
  from `playbooks/`, then launch 5 parallel agents.

### `ideas/`

- **`G2-INSPIRED-SAAS-IDEAS.md`** — SaaS ideas inspired by G2 categories.

### `ops/`

- **`DISK-CLEANUP-PLAN.md`** — Dev-machine disk hygiene.
- **`TRUSTED-RESOURCES-GUIDE.md`** — Vetted external resources.

---

## How Cursor Discovers This

1. **Skill** at `C:\Users\PGI\.cursor\skills\pgi-knowledge\SKILL.md` tells every Cursor
   agent (in any project) to consult this folder for cross-project knowledge.
2. **Per-project junction** (optional) at `<project>\docs\global` → `D:\vitou\knowledge\`
   so you can `@docs/global/strategies/...` in chat. Create with:
   ```powershell
   mklink /J docs\global D:\vitou\knowledge
   ```
3. **`AGENTS.md`** in this folder ships the global agent rules.

---

## Maintenance Rules

- **Generic only.** No project-specific names, IPs, secrets, or schema details belong here.
  Project-specific docs stay inside the project's `docs/`.
- **Edit in place.** This is the single source of truth. Do not duplicate into project
  folders — junction or `@`-reference instead.
- **Date your changes.** Add a `> Updated: YYYY-MM-DD` line at the top of any doc you edit.
- **Generalize aggressively.** When a one-off project doc proves useful twice, strip the
  specifics and promote it into `playbooks/` or `stacks/`.
