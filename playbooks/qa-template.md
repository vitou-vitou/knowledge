# Playbook 04 — QA Agent

> Fork into `<project>/docs/agents/04-qa-<area>.md`. Replace every `[PLACEHOLDER]`.
> Source: `D:\vitou\knowledge\playbooks\qa-template.md`

**Model:** `composer-2-fast` (efficient for repetitive test code)
**Scope (allowed files):**

- `tests/**` — **new files only**, do not modify existing test files
- `[MODULE_PATH]/Tests/**` — **new files only**
- `docs/agents/handoff/04-qa-handoff.md`

**Do NOT touch:** any source code under `app/`, `[MODULE_PATH]/Http/`,
`[MODULE_PATH]/Services/`, `[MODULE_PATH]/Resources/`, `config/`, `routes/`, `.env*`,
`composer.json`, `package.json`, `phpunit.xml`. If the test config needs an extra suite
path, propose it in the handoff — do not edit it yourself.

---

## Background

Stack: `[PHP/Node/Python] [VERSION], [FRAMEWORK] [VERSION], [TEST_FRAMEWORK] [VERSION]`.
Module under test: `[MODULE_PATH]`. Key controllers: `[CONTROLLERS]`.
Special data shape (e.g. union view, denormalized cache): `[NOTE]`.

## Your Mission

Write **fast, isolated, deterministic** feature tests for `[FEATURE_AREA]`. You do not
need to actually run them in this session (no test DB is guaranteed available) — write
them so they would pass against a configured test DB.

1. **Test file layout**
   - `[MODULE_PATH]/Tests/Feature/[Area]ListTest.php`
   - `[MODULE_PATH]/Tests/Feature/[Area]ExportTest.php`
   - `[MODULE_PATH]/Tests/Feature/[Area]BulkActionTest.php` (covers Agent 02's contract;
     mark with `@group pending-feature` if it lands after this run)
   - `tests/Feature/[Module]/HealthRouteTest.php` — smoke test that the module loads.

2. **What to cover**
   - Happy path: list/show returns 200 for an authenticated user, 401/302 for unauth.
   - Filter / search / date range narrows results correctly.
   - Export / report endpoint returns correct headers + valid file structure.
   - Empty-result export returns 200 with valid headers, not 500.
   - Bulk action endpoint validates inputs and rejects invalid actions.

3. **Style**
   - Use `RefreshDatabase` only if a test DB is configured; otherwise `markTestSkipped()`
     so the suite never blows up on a dev box.
   - Read existing tests under `tests/` first to match the project's auth-acting pattern
     (Sanctum / Passport / standard guard).
   - Each test independent. No reliance on test ordering.

4. **Documentation**
   - In `docs/agents/handoff/04-qa-handoff.md`:
     - Exactly how to run the new suites.
     - Required env keys (`DB_*`, etc.) and a note that external services
       (LDAP / OAuth / payment / SMS) must be mocked or skipped.
     - Recommended mocks for this project.

## Constraints

- Do not modify existing tests.
- Do not run package managers or test runners.
- Do not edit test config — propose changes in handoff.
- Leave changes uncommitted.

## Definition of Done

- New test files exist, syntactically valid, namespaced correctly, with the project's
  naming convention (`@test` annotation or `test_*` method names — match existing).
- Each test has a clear arrange / act / assert structure.
- Handoff doc written with how-to-run + recommended mocks.
