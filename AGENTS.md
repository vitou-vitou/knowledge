# Global Agent Instructions

> These rules apply to any Cursor agent working on a project under `D:\vitou\projects\`.
> Project-specific `AGENTS.md` may extend or override these rules.

---

## 1. Knowledge Base

The authoritative reference for cross-project methods, stacks, and playbooks lives at:

```
D:\vitou\knowledge\
```

Before designing any non-trivial multi-step plan, multi-agent run, or new project
scaffold, **read the relevant docs there first**. Index is at
`D:\vitou\knowledge\README.md`. Most-used:

- `strategies\SAAS-MULTI-AGENT-STRATEGY.md` — parallel 5-agent method
- `strategies\PARALLEL-AGENTS-ADVANCED.md` — coordination patterns
- `strategies\MODELS-RANKING.md` — model selection by task
- `playbooks\*-template.md` — fork to author per-project agent specs

---

## 2. Tone & Output

- Terse. High signal per token. No marketing language.
- No emojis unless the user explicitly asks.
- No filler comments in code (`// Increment counter`, etc.). Only comments that explain
  non-obvious intent or constraints.
- Cite existing code with `startLine:endLine:filepath` blocks. Cite proposed code with
  fenced blocks tagged with the language only.

---

## 3. Multi-Agent Runs

When the user asks for a parallel multi-agent run:

1. **Always write task specs as `.md` files first** under `docs\agents\` in the project,
   one per agent (`01-*.md` … `05-*.md`).
2. **Forks come from `D:\vitou\knowledge\playbooks\*-template.md`** — strip placeholders,
   fill in project specifics.
3. **Scopes must not overlap.** Each spec lists allowed files explicitly. Architect/QA
   typically only write new docs/tests; bugfix and feature touch source.
4. **Always include**: model slug, allowed files, forbidden actions
   (no composer/npm/artisan/commit), handoff doc path.
5. **Launch all agents in a single message** with parallel Task calls. `run_in_background: true`.
6. **Use available model slugs only.** If the user picks one not in the available list,
   tell them which is unavailable rather than silently substituting.

---

## 4. Project Hygiene

- **Never commit** unless the user explicitly asks.
- **Never run** `composer install`, `npm install`, `php artisan migrate`, or
  `docker build` without explicit instruction.
- **Never edit** `.env` (live env). `.env.example` is fair game when scoped to a DevOps
  task.
- **Preserve typos in identifiers** (e.g. `TplClientControler.php` — single `l` —
  may be load-bearing). Confirm before renaming.
- **Stay on the user's current branch.** Don't switch branches autonomously.

---

## 5. When Stuck

- Don't loop. After two failed attempts at the same fix, write a short `STUCK.md` note
  describing what you tried, what failed, and what you'd try next, then ask the user.
- Prefer `AskQuestion` (multiple choice) over open-ended clarification when a decision
  has < 5 plausible answers.
