# Playbook 02 — Feature Agent

> Fork into `<project>/docs/agents/02-feature-<name>.md`. Replace every `[PLACEHOLDER]`.
> Source: `D:\vitou\knowledge\playbooks\feature-template.md`

**Model:** `claude-4.6-sonnet-medium-thinking` (balances quality + speed for substantive UI work)
**Scope (allowed files):**

- `[VIEWS_FOLDER]/**`
- `[ASSETS_FOLDER]/**` (JS / CSS / Sass)
- `[ROUTES_FILE]` — **add** routes only, do not modify existing
- `[CONTROLLER_PATH]` — **append** new methods only; do not modify existing methods
  (Agent 01 owns those)

**Do NOT touch:** anything outside the list above. Especially:
- existing controller methods (Agent 01 territory)
- export / transform code (Agent 01 territory)
- `.env*`, `config/**`, `composer.json`, `package.json`, migrations, `tests/`

---

## Background

Short paragraph on what the user / team needs and why now:

> [WHY NOW, e.g. "Operators routinely process many rows at once but every action today is single-row."]

## Your Mission

Add **`[FEATURE_NAME]`** to `[FEATURE_LOCATION]`, scoped tightly:

1. **`[CAPABILITY_1]`** — [1-line spec]
2. **`[CAPABILITY_2]`** — [1-line spec]
3. **`[CAPABILITY_3]`** — [1-line spec]
4. **UX polish (in scope)**
   - [Spec 1]
   - [Spec 2]
   - Loading state on async actions.
   - Toast on success/failure (use existing toast lib; no new npm deps).
   - Keyboard shortcuts: [LIST].

5. **Routes**
   - Add a single new route `[METHOD] [PATH]` mapped to a new controller method.
   - Validate inputs strictly. Document the contract in the handoff so QA (Agent 04) can
     test it.

## Hand-off Contract

Write `docs/agents/handoff/02-feature-handoff.md` listing:
- New routes added.
- New controller method signatures.
- Any UX item skipped + reason.
- Any place you needed Agent 01 / Agent 03 to add support — flag explicitly. Do not edit
  outside your scope yourself.

## Constraints

- No new package dependencies. Match the existing visual / JS conventions of the layout.
  Inspect before injecting.
- Do not rename load-bearing files.
- Do not run package managers or build tools.
- Leave changes uncommitted.

## Definition of Done

- Feature works on initial load and after pagination / filter change.
- No console errors. No layout jitter. No lint errors in touched files.
- Handoff doc written.
