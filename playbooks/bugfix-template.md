# Playbook 01 — Bugfix Agent

> Fork into `<project>/docs/agents/01-bugfix-<topic>.md`. Replace every `[PLACEHOLDER]`.
> Source: `D:\vitou\knowledge\playbooks\bugfix-template.md`

**Model:** `gpt-5.3-codex-high-fast` (preferred for backend code generation)
**Scope (allowed files):**

- `[PRIMARY_CONTROLLER_PATH]`
- `[SECONDARY_CONTROLLER_OR_SERVICE_PATH]`
- `[EXPORT_OR_TRANSFORM_FOLDER]/**`
- `[VIEW_FOLDER_FOR_THIS_FEATURE]/**` (only blade snippets directly related to the bug)
- `[ROUTES_FILE]` (only if a missing route is the bug)

**Do NOT touch:** anything else. Especially `.env*`, `config/**`, `composer.json`,
`package.json`, migrations, the `tests/` folder, other modules.

---

## Background

Recent commits hitting this area:

```
[PASTE 3-5 RELEVANT git log --oneline LINES HERE]
```

Short paragraph on the symptom and where the user has seen it:

> [SYMPTOM, e.g. "Export rows don't match the on-screen list after the union view change."]

## Your Mission

1. **Audit** the affected flow end-to-end:
   - The action(s) inside `[PRIMARY_CONTROLLER_PATH]`.
   - Any `[EXPORT_OR_TRANSFORM_FOLDER]` classes used.
   - The query builder used to render the on-screen view vs. the one used to build the
     export / transform / report.

2. **Find and fix** at minimum these classes of bugs (and any others you discover):
   - Query divergence between screen and export/report.
   - Filters / search / date range applied on screen but not on export.
   - Headings or mapped columns drifted from the table.
   - Memory blow-ups on large datasets — switch to chunking / `FromQuery` if needed.
   - Type / format issues (leading zeros, phone numbers, dates, multi-byte text).
   - Empty-result path returns an exception instead of a valid empty response.

3. **Verify** by reading the corresponding view(s) and matching visible columns 1:1 to the
   export / transform output.

4. **Output a short report** at `docs/agents/handoff/01-bugfix-handoff.md` with:
   - Each bug found (1 line each).
   - The fix applied (`file:line`).
   - Anything you noticed but did not fix (out of scope or risky).

## Constraints

- Stack: `[PHP/Node/Python] [VERSION], [FRAMEWORK] [VERSION]`.
- Do not rename load-bearing files (check with the user first if unsure).
- Do not change DB schema or add migrations.
- Do not run package managers or build tools.
- Leave changes uncommitted.

## Definition of Done

- Output matches on-screen view 1:1 under the same filters.
- Empty-result path returns a valid response.
- Large-dataset path does not exhaust memory.
- Handoff doc written.
