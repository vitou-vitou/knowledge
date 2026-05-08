# Playbook 05 — Architect Agent (Read-Only)

> Fork into `<project>/docs/agents/05-architect-<scope>.md`. Replace every `[PLACEHOLDER]`.
> Source: `D:\vitou\knowledge\playbooks\architect-template.md`

**Model:** `claude-opus-4-7-thinking-xhigh` (deep reasoning, complex planning)
**Mode:** **Read-only.** Write only inside `docs/agents/architect/**`.

**Scope (allowed files to write):**

- `docs/agents/architect/MODULE-MAP.md`
- `docs/agents/architect/DATA-FLOW.md`
- `docs/agents/architect/RISK-REGISTER.md`
- `docs/agents/architect/REFACTOR-BACKLOG.md`
- `docs/agents/handoff/05-architect-handoff.md`

**Do NOT touch:** any source code anywhere. Any config. Any route. Any test. Any `.env*`.
The deliverable is documentation only.

---

## Background

`[PROJECT_NAME]` is `[ONE_PARAGRAPH_DESCRIPTION]`. Stack: `[STACK]`. Integrations:
`[LIST_INTEGRATIONS]`. Recent area of activity: `[RECENT_FOCUS]`.

## Your Mission

Produce a **terse, high-signal** architect-level audit. No fluff. No marketing language.

1. **`MODULE-MAP.md`** — One-page map of `[PRIMARY_MODULE_PATH]`:
   - Each subfolder + its purpose.
   - For each Controller: 1-line summary + the routes it serves.
   - For each Service: 1-line summary.
   - Mark dead-looking code with `(?dead)`.

2. **`DATA-FLOW.md`** — ASCII flow for the `[CORE_LIFECYCLE]`:
   - Where data originates (forms / imports / external APIs).
   - How it transforms through services.
   - How it's persisted (which tables, which connections).
   - How the read path differs from the write path.
   - External integration touchpoints.

3. **`RISK-REGISTER.md`** — Top 10 risks, ordered by severity. For each:
   - 1-line description.
   - File path (no need for line numbers).
   - Severity: `critical | high | medium | low`.
   - 1-line mitigation.
   - Look for: hard-coded secrets, hard-coded internal IPs, missing HTTP timeouts/retries,
     missing rate limits on auth/OTP, N+1, mass-assignment risks, role checks in
     controllers vs middleware, single huge controllers, OS-specific paths from app code.

4. **`REFACTOR-BACKLOG.md`** — Top 10 refactor candidates by ROI. For each:
   - What to refactor.
   - Why (1 line).
   - Effort: `S | M | L`.
   - Risk: `S | M | L`.
   - Suggested first commit.

5. **`docs/agents/handoff/05-architect-handoff.md`** — Index of the four docs above + a
   3-bullet executive summary aimed at the team lead.

## Constraints

- **Do not edit any source file. Period.** If you catch yourself opening write tools on a
  non-doc path, stop.
- This team ships frequently — propose **incremental, reversible** refactors, not rewrites.
- Use domain-specific language (`[DOMAIN_TERMS]`) — assume the reader knows it.
- Do not run any state-mutating tool.

## Definition of Done

- All five documents exist, each ≤ 2 pages.
- Handoff doc has the 3-bullet exec summary at the top.
- Zero source files modified.
