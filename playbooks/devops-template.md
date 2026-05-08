# Playbook 03 — DevOps Agent

> Fork into `<project>/docs/agents/03-devops-<topic>.md`. Replace every `[PLACEHOLDER]`.
> Source: `D:\vitou\knowledge\playbooks\devops-template.md`

**Model:** `gpt-5.5-medium` (broad knowledge for tooling + infra)
**Scope (allowed files):**

- `.env.example`
- `config/[RELEVANT_CONFIG].php`
- `docker/**`
- `Dockerfile`, `Dockerfile.*`, `.dockerignore`
- `render.yaml` / `fly.toml` / `vercel.json` if present
- `docs/agents/handoff/03-devops-handoff.md`

**Do NOT touch:** any source under `app/`, `Modules/`, `routes/`, `database/`, `tests/`.
No blade or JS. No `composer.json` / `package.json`. No live `.env`.

---

## Background

Current state of the relevant config files:

```
[git status snippet OR brief description]
```

Known pain points:
- [PAIN 1, e.g. ".env.example has duplicate blocks with conflicting Linux/Windows values"]
- [PAIN 2]

## Your Mission

1. **Deduplicate / clean `[CONFIG_FILE]`** without dropping any unique key.
   - Group by section with comment headers (`#--- Database ---`, `#--- Mail ---`, etc.).
   - When the same key appears twice with different values, keep the production / Linux /
     deployed value as the active one and move the other under a `# Local fallback` block.
     Do not delete — comment.
   - Quote any value containing `#`, `$`, spaces, or backslashes.
   - Preserve any tooling markers (e.g. `#<Refund>...#</Refund>`).
   - [PROJECT-SPECIFIC ORDER SUGGESTION]

2. **`config/[RELEVANT_CONFIG].php` review.** Comment-only changes unless the user
   explicitly asks for behavior changes.

3. **`docker/`, `Dockerfile`, `.dockerignore` review.**
   - List every issue in the handoff.
   - Only fix **safe** issues: missing `.env` ignore, missing `node_modules` ignore,
     wrong language version vs `[VERSION_FILE]`, missing extensions for the integrations
     this app uses (e.g. `oci8` / `pdo_oci` for Oracle, `redis`, `gd`, `imagick`).
   - For risky changes (base image swap, package install) — **propose in handoff**, do
     not change.

4. **`render.yaml` / equivalent** — if present, audit. If absent, do not create one.

## Constraints

- Do not change config behavior — comment-only.
- Do not delete environment keys — deduplicate or comment as fallback.
- Do not run `composer`, `npm`, `php artisan`, `docker build`, etc.
- Leave changes uncommitted.

## Definition of Done

- Zero duplicate keys (or every duplicate is intentional + commented as a fallback).
- Required quoting applied.
- Handoff doc lists every duplicate removed, every conflict reconciled, every Docker issue
  (fixed or proposed), and a before/after line count of the cleaned config file.
