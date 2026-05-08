# AutoSale - Complete Reference

> Branch: `features/auto`
> Workspace: `D:/g4vt/gpi-agency-portal`
> Knowledge base scanned: `D:/knowledge/knowledge` (a.k.a. `D:\vitou\knowledge\`)
> Generated: 2026-05-08

---

## 0. Executive Summary (Read This First)

**No AutoSale references found in knowledge base.**

Every file under `D:/knowledge/knowledge/` was enumerated and searched. The knowledge
base is a **generic, cross-project methodology/playbook archive** about multi-agent
orchestration, model selection, and Laravel + Filament + Livewire stack patterns.
It contains **no business-domain documentation** about an "AutoSale", "Auto Sale",
"automated sale", "auto booking", or any equivalent automated-commerce flow.

Per the project's own rules (`D:/knowledge/knowledge/README.md`):

> **Generic only.** No project-specific names, IPs, secrets, or schema details belong here.
> Project-specific docs stay inside the project's `docs/`.

That means AutoSale documentation, if it exists, lives **inside this project**
(`D:/g4vt/gpi-agency-portal/`) under its own `docs/`, source code, OpenSpec changes,
or Filament resources - **not** in the global knowledge base. This document records
that finding exhaustively, plus a tooling-grounded skeleton that the next agent can
fill in from project-local sources.

---

## 1. Search Methodology (What Was Tried)

### 1.1 Enumeration
Used `Glob` with `target_directory: "D:/knowledge/knowledge"` and a recursive
`Get-ChildItem -Recurse -Include *.md`. Total markdown files found: **17**
(plus `.gitignore` and an internal `.git/` folder, both irrelevant). All 17 markdown
files were read in full and scanned.

### 1.2 Search Patterns (case-insensitive)
The following regex patterns were run against every `.md` file in the knowledge base:

| # | Pattern | Tool | Hits |
|---|---|---|---|
| 1 | `auto[\s\-]?sale` | ripgrep / Select-String | 0 |
| 2 | `automated sale` | ripgrep / Select-String | 0 |
| 3 | `auto[\s\-]?book` | ripgrep / Select-String | 0 |
| 4 | `autosale` | ripgrep / Select-String | 0 |
| 5 | `auto.{0,5}sale` | ripgrep / Select-String | 0 |
| 6 | `auto.{0,3}sale\|automated sale\|autosale\|auto.{0,3}book` | Select-String | 0 |
| 7 | Bare `sale` (ANY occurrence anywhere) | Select-String | 0 |
| 8 | `auto` | Select-String | 8 (none AutoSale-related - see 1.3) |
| 9 | `sale\|booking\|commerce` | Select-String | 0 (booking/commerce); for `sale` see 1.3 |
| 10 | `auto\|sale\|book\|order\|payment\|invoice\|commission\|trigger\|workflow\|automat` | Select-String | many, none AutoSale-related |

### 1.3 The only "auto-" / "sale-" matches in the entire knowledge base
None describe a business AutoSale flow. They are:

- **`auto-disable`** - Cursor model UI auto-disable behaviour (`PARALLEL-AGENTS-ADVANCED.md:14`).
- **`Auto mode`** - Cursor model selector "Auto" mode (`MODELS-RANKING.md:89,114,116,125`).
- **`AutoGen`** - Microsoft's multi-agent framework (`TRUSTED-RESOURCES-GUIDE.md:43,249`).
- **`Format-AutoSize`** - PowerShell formatting cmdlet (`DISK-CLEANUP-PLAN.md:162,202,209,216`).
- **`auto-scaling`** - Generic SaaS infra term (`G2-INSPIRED-SAAS-IDEAS.md:198`).
- **`composer install --no-dev --optimize-autoloader`** - Composer flag (`LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md:288`).
- **`Automated backups`** / **`spatie/laravel-backup`** - Laravel package (`LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md:246`).
- **`automatic transcription`** / **`Auto-organize meeting insights`** - Project 5 SaaS idea (meeting notes assistant) in `G2-INSPIRED-SAAS-IDEAS.md:210,227`.
- **`Salesforce`** / **`HubSpot Marketing automation`** - SaaS market examples in `G2-INSPIRED-SAAS-IDEAS.md:13,16,23,250`.
- **`autocomplete`** - editor feature (`MODELS-RANKING.md:16,72`).

None of these references describe an "AutoSale" business workflow.

### 1.4 What the knowledge base actually contains (by folder)

| Folder | Files | Topic |
|---|---|---|
| `strategies/` | 4 | Multi-agent SaaS strategy, parallel agent coordination, model ranking, chat-continuation phrases |
| `stacks/laravel/` | 2 | Laravel + Filament + Livewire 5-agent build playbook, quick-launch template |
| `playbooks/` | 5 | Forkable role specs: bugfix, feature, devops, qa, architect (placeholders only) |
| `templates/agent-run/` | 1 | README explaining how to fork the playbooks into `<project>/docs/agents/` |
| `ideas/` | 1 | G2-inspired SaaS project ideas (invoice gen, task tracker, expense tracker, URL shortener, meeting notes) |
| `ops/` | 2 | Disk cleanup runbook, trusted-resources guide (CrewAI, LangGraph, AutoGen, MERN boilerplates) |
| root | 2 | `README.md` (index), `AGENTS.md` (global agent rules) |

None reference a sales / commerce / booking domain by name.

---

## 2. AutoSale Definition (Per Knowledge Base)

**Not defined in the knowledge base.**

Future agents should look for the AutoSale definition in these likely
project-internal locations instead:

- `D:/g4vt/gpi-agency-portal/openspec/` - OpenSpec changes/specs (the project ships
  several OpenSpec skill files; see `Available Skills`).
- `D:/g4vt/gpi-agency-portal/docs/` - Project-specific documentation.
- `D:/g4vt/gpi-agency-portal/app/Filament/` - Filament resources for any AutoSale entity.
- `D:/g4vt/gpi-agency-portal/app/Models/` - Eloquent models named e.g. `AutoSale`, `Sale`, `Booking`.
- `D:/g4vt/gpi-agency-portal/app/Jobs/` - Queued jobs that drive automation.
- `D:/g4vt/gpi-agency-portal/app/Services/` - Sale orchestration services.
- `D:/g4vt/gpi-agency-portal/database/migrations/` - Schema for sale-related tables.
- `D:/g4vt/gpi-agency-portal/routes/` - HTTP/API entry points.

The companion document `Project.md` (being written concurrently in this same
`features/auto/` folder by another agent) is expected to contain the project-level
context required to fill in section 3 onward.

---

## 3. Step-by-Step Workflow

**Not specified in the knowledge base.** No step ordering, decision branches, or
sub-steps for an AutoSale flow appear anywhere in `D:/knowledge/knowledge/`.

When the next agent fills this in from project-internal sources, the recommended
template (lifted from `playbooks/architect-template.md`'s `DATA-FLOW.md` brief at
`D:/knowledge/knowledge/playbooks/architect-template.md:37-43`) is:

> ASCII flow for the [CORE_LIFECYCLE]:
> - Where data originates (forms / imports / external APIs).
> - How it transforms through services.
> - How it's persisted (which tables, which connections).
> - How the read path differs from the write path.
> - External integration touchpoints.

A blank skeleton to populate (do not invent values - leave `???` until verified
against project source):

```
Trigger ???        ->  Validation ???        ->  Persistence ???
   |                       |                          |
   v                       v                          v
Job dispatch ???   ->  External call ???    ->  Status update ???
                                                     |
                                                     v
                                              Notification ???
```

---

## 4. Actors / Roles / Permissions / System Components

**Not specified in the knowledge base.** The only role inventory present in the
knowledge base is the **5-agent build team** for greenfield SaaS projects, which is
about *who builds the software* and not *who uses an AutoSale flow*:

> | Agent Role | Model | Specialization | Deliverable |
> |---|---|---|---|
> | Architect | claude-opus-4-7-thinking-xhigh | System design, data flow, tech decisions | Architecture doc + project structure |
> | Frontend  | claude-4.6-sonnet-medium-thinking | UI/UX, React components, styling | Complete frontend codebase |
> | Backend   | gpt-5.3-codex | API routes, database, auth, business logic | Complete backend codebase |
> | DevOps    | gpt-5.5-medium | Docker, deployment, CI/CD, environment | Infrastructure & deployment code |
> | QA/Polish | composer-2-fast | Testing, bug fixes, docs, final touches | Test suite + documentation |
>
> -- `D:/knowledge/knowledge/strategies/SAAS-MULTI-AGENT-STRATEGY.md`

Permissions guidance for **production Filament apps** (still generic, not AutoSale-specific)
is in `D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`:

> ### Filament Security
> - Authentication middleware on all admin routes
> - Permission-based navigation hiding
> - Resource-level authorization policies
> - Audit logging for admin actions
>
> -- lines 343-348

Recommended packages for permission management (also from the same file, lines 244, 254):

> - `spatie/laravel-permission` - Role & permission management
> - `bezhansalleh/filament-shield` - Permission management UI

---

## 5. Inputs, Outputs, Data Fields, Validation, Configuration

**Not specified in the knowledge base.** No field-level schema for a Sale, Booking,
Commission, or AutoSale entity exists anywhere in `D:/knowledge/knowledge/`.

The knowledge base only describes *generic* schema-design tasks the Architect agent
should perform. From `D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md:136-141`:

> ### Database Design
> - Use migrations for all schema changes
> - Define relationships in models clearly
> - Implement soft deletes where appropriate
> - Use factories for testing data

For inputs / validation, the project should follow the standard Laravel
`Http/Requests/*.php` pattern (per the same file, lines 32-37):

> Key Outputs:
> - `app/Http/Controllers/*.php`
> - `app/Services/*.php`
> - `app/Http/Requests/*.php`
> - `database/seeders/*.php`
> - `app/Jobs/*.php`

---

## 6. Triggers, Schedules, Queues / Jobs, Retries, Idempotency, Failure Handling

**Not specified in the knowledge base.** No scheduling, queue topology, retry policy,
or idempotency contract for AutoSale exists in the consulted files.

Generic Laravel guidance from `D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`:

> ### Performance Optimization
> - Eager loading to prevent N+1 queries
> - Caching for frequently accessed data
> - **Queue long-running tasks**
> - Optimize Livewire component rendering
>
> -- lines 154-158

> ### Laravel-Specific Monitoring
> // Laravel Telescope (development)
> // Laravel Horizon (queue monitoring)
> // Laravel Pulse (application monitoring)
> // Sentry Laravel integration (error tracking)
>
> -- lines 358-364

That is the extent of what the knowledge base says about queues; nothing is
AutoSale-specific.

---

## 7. State Transitions, Statuses, Edge Cases

**Not specified in the knowledge base.** No state machine, status enum, or
edge-case catalogue for AutoSale appears in any consulted file.

The closest *generic* edge-case checklist is from the bugfix playbook
`D:/knowledge/knowledge/playbooks/bugfix-template.md:40-46`:

> Find and fix at minimum these classes of bugs (and any others you discover):
> - Query divergence between screen and export/report.
> - Filters / search / date range applied on screen but not on export.
> - Headings or mapped columns drifted from the table.
> - Memory blow-ups on large datasets - switch to chunking / FromQuery if needed.
> - Type / format issues (leading zeros, phone numbers, dates, multi-byte text).
> - Empty-result path returns an exception instead of a valid empty response.

Apply these as smoke checks once the AutoSale flow is implemented; they are not
AutoSale-specific definitions.

---

## 8. Integration Points

**Not specified in the knowledge base.** No mention of which Filament panels,
models, services, jobs, events, APIs, or third parties participate in AutoSale.

The Filament integration *pattern* (still generic) is from
`D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md:142-147`:

> ### Filament Integration
> - One resource per model as baseline
> - Custom pages for complex workflows
> - Widgets for dashboard insights
> - Custom fields for specific needs

Filament file conventions (lines 47-52):

> Key Outputs:
> - `app/Filament/Resources/*.php`
> - `app/Filament/Pages/*.php`
> - `app/Filament/Widgets/*.php`
> - Custom Filament components

Recommended Filament ecosystem packages (lines 250-254):

> - `filament/spatie-laravel-media-library-plugin` - File uploads
> - `filament/spatie-laravel-settings-plugin` - Application settings
> - `filament/spatie-laravel-translatable-plugin` - Multi-language
> - `bezhansalleh/filament-shield` - Permission management UI

None of these are AutoSale-specific.

---

## 9. Policies, Business Rules, Pricing/Commission, Compliance

**Not specified in the knowledge base.** No pricing model, commission tier, refund
rule, or compliance requirement for AutoSale appears anywhere in the consulted files.

The project's `AGENTS.md` (`D:/knowledge/knowledge/AGENTS.md:62-66`) does ship one
hygiene rule that is relevant whenever AutoSale flows touch config files:

> - **Preserve typos in identifiers** (e.g. `TplClientControler.php` - single `l` -
>   may be load-bearing). Confirm before renaming.

If the AutoSale code references load-bearing legacy identifiers, do not rename them
without explicit confirmation.

---

## 10. UI References, Screens, Commands

**Not specified in the knowledge base.** No screenshot, screen wireframe, or
artisan command name tied to AutoSale exists in the consulted files.

The Livewire UI *pattern* (generic) is from
`D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md:148-152`:

> ### Livewire Patterns
> - Single Responsibility per component
> - Use wire:model for two-way binding
> - Implement loading states for UX
> - Events for component communication

For new feature UI work, the feature playbook
`D:/knowledge/knowledge/playbooks/feature-template.md:35-41` lists the UX
checklist a feature agent must satisfy:

> 4. UX polish (in scope)
>    - [Spec 1]
>    - [Spec 2]
>    - Loading state on async actions.
>    - Toast on success/failure (use existing toast lib; no new npm deps).
>    - Keyboard shortcuts: [LIST].

---

## 11. Related Playbooks, Ops Procedures, Templates

The closest the knowledge base gets to AutoSale is **how to staff a 5-agent build
that *would* implement AutoSale** if a project asked for one. The forkable
playbooks are:

| ID | Playbook | Model | Source path |
|---|---|---|---|
| 01 | Bugfix | `gpt-5.3-codex-high-fast` | `D:/knowledge/knowledge/playbooks/bugfix-template.md` |
| 02 | Feature | `claude-4.6-sonnet-medium-thinking` | `D:/knowledge/knowledge/playbooks/feature-template.md` |
| 03 | DevOps | `gpt-5.5-medium` | `D:/knowledge/knowledge/playbooks/devops-template.md` |
| 04 | QA | `composer-2-fast` | `D:/knowledge/knowledge/playbooks/qa-template.md` |
| 05 | Architect (read-only) | `claude-opus-4-7-thinking-xhigh` | `D:/knowledge/knowledge/playbooks/architect-template.md` |

> Source: `D:/knowledge/knowledge/templates/agent-run/README.md:20-26`

To start an AutoSale-specific multi-agent build, fork the playbooks per the rules
in `D:/knowledge/knowledge/AGENTS.md:38-52`:

> When the user asks for a parallel multi-agent run:
> 1. Always write task specs as .md files first under `docs/agents/` in the project,
>    one per agent (`01-*.md` ... `05-*.md`).
> 2. Forks come from `D:/vitou/knowledge/playbooks/*-template.md` - strip placeholders,
>    fill in project specifics.
> 3. Scopes must not overlap. Each spec lists allowed files explicitly. Architect/QA
>    typically only write new docs/tests; bugfix and feature touch source.
> 4. Always include: model slug, allowed files, forbidden actions
>    (no composer/npm/artisan/commit), handoff doc path.
> 5. Launch all agents in a single message with parallel Task calls.
>    `run_in_background: true`.
> 6. Use available model slugs only.

The conflict-resolution hierarchy when multiple agents touch the AutoSale flow
(from `D:/knowledge/knowledge/strategies/PARALLEL-AGENTS-ADVANCED.md:69-83`):

> | Priority | Agent | Reasoning |
> |:---:|---|---|
> | 1 | Architect | System design decisions are foundational |
> | 2 | Backend | Data models affect everything else |
> | 3 | Frontend | UI/UX decisions impact user experience |
> | 4 | DevOps | Infrastructure adapts to application needs |
> | 5 | QA | Tests adapt to final implementation |
>
> Common Conflicts:
> - API mismatch -> Backend wins, Frontend adapts
> - Database schema -> Backend wins, Frontend queries adapt
> - Auth strategy -> Architect wins, all others follow
> - Deployment method -> DevOps wins, others provide requirements

---

## 12. Hard Constraints When Touching the AutoSale Code Path

Lifted directly from the project's global agent rules
(`D:/knowledge/knowledge/AGENTS.md:56-66`) and the agent-run README
(`D:/knowledge/knowledge/templates/agent-run/README.md:28-39`). These apply
**verbatim** to any agent implementing or modifying AutoSale:

1. **Never commit unless the user explicitly asks.**
2. **Never run** `composer install`, `npm install`, `php artisan migrate`, or
   `docker build` without explicit instruction.
3. **Never edit** `.env` (live env). `.env.example` is fair game when scoped to a
   DevOps task.
4. **Preserve typos in identifiers** (e.g. `TplClientControler.php` - single `l` -
   may be load-bearing). Confirm before renaming.
5. **Stay on the user's current branch.** Don't switch branches autonomously.
6. **Stay within your scope.** If you need to change a file outside your scope,
   write a note in `docs/agents/handoff/<your-id>-handoff.md` instead of editing.
7. **Do not touch** `composer.json`, `composer.lock`, `package.json`,
   `package-lock.json` unless your task explicitly requires it.
8. **Commit nothing.** Leave changes in the working tree for the user to review.

---

## 13. Recommended Next Actions for the Following Agent

Because the knowledge base supplies process but not domain content, the next agent
needs to extract AutoSale specifics from the project itself before this document
can be made truly exhaustive. Suggested order:

1. Read `D:/g4vt/gpi-agency-portal/features/auto/Project.md` (the sibling
   document being written concurrently) for project-level context.
2. `Glob` for AutoSale-named artifacts inside the workspace:
   - `**/AutoSale*.php`
   - `**/*Sale*.php` (Models, Resources, Services, Jobs, Policies, Events)
   - `**/*Booking*.php`
   - `app/Filament/**/*Sale*`
   - `database/migrations/*sale*`
   - `routes/**/*` for any `auto-sale`, `auto_sale`, or `sale` route names
   - `openspec/**/*sale*` and `openspec/**/*auto*`
3. Read those files in full and re-fill sections 2 through 11 of this document
   with verified project-source content (not invented values).
4. Apply the Architect playbook (`architect-template.md`) deliverables
   (`MODULE-MAP.md`, `DATA-FLOW.md`, `RISK-REGISTER.md`, `REFACTOR-BACKLOG.md`)
   scoped specifically to the AutoSale flow once the source has been mapped.
5. Apply the QA playbook (`qa-template.md`) to add feature tests under
   `tests/Feature/AutoSale/` that cover happy path, empty-result, bulk action,
   and validation paths.

---

## 14. What This Document Does *Not* Cover

To be unambiguous: this document does **not** contain any of the following items
because they are absent from `D:/knowledge/knowledge/`:

- AutoSale's business definition.
- The AutoSale step-by-step workflow.
- AutoSale's data fields, validation rules, or configuration knobs.
- AutoSale's queue / job / scheduling / retry / idempotency policy.
- AutoSale's state machine and statuses.
- AutoSale's integration points (Filament panels, models, services, events, APIs, third parties).
- AutoSale's pricing / commission / compliance rules.
- AutoSale's UI screens, routes, or artisan commands.

Every one of these has to be sourced from project-local code/docs and inserted by
the next agent. This document is **complete** with respect to the global knowledge
base; it is **deliberately incomplete** with respect to the project-local AutoSale
implementation, because no project-local context was in scope for this run.

---

## 15. Sources (every knowledge file consulted)

All paths are absolute. All `.md` files in `D:/knowledge/knowledge/` were read in
full; none mention AutoSale.

| # | Absolute path | Read in full | AutoSale hits |
|---|---|:---:|:---:|
| 1 | `D:/knowledge/knowledge/README.md` | yes | 0 |
| 2 | `D:/knowledge/knowledge/AGENTS.md` | yes | 0 |
| 3 | `D:/knowledge/knowledge/strategies/SAAS-MULTI-AGENT-STRATEGY.md` | yes | 0 |
| 4 | `D:/knowledge/knowledge/strategies/PARALLEL-AGENTS-ADVANCED.md` | yes | 0 |
| 5 | `D:/knowledge/knowledge/strategies/MODELS-RANKING.md` | yes | 0 |
| 6 | `D:/knowledge/knowledge/strategies/HOW-TO-CONTINUE-CHATS.md` | yes | 0 |
| 7 | `D:/knowledge/knowledge/stacks/laravel/LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md` | yes | 0 |
| 8 | `D:/knowledge/knowledge/stacks/laravel/LARAVEL-QUICK-LAUNCH.md` | yes | 0 |
| 9 | `D:/knowledge/knowledge/playbooks/bugfix-template.md` | yes | 0 |
| 10 | `D:/knowledge/knowledge/playbooks/feature-template.md` | yes | 0 |
| 11 | `D:/knowledge/knowledge/playbooks/devops-template.md` | yes | 0 |
| 12 | `D:/knowledge/knowledge/playbooks/qa-template.md` | yes | 0 |
| 13 | `D:/knowledge/knowledge/playbooks/architect-template.md` | yes | 0 |
| 14 | `D:/knowledge/knowledge/templates/agent-run/README.md` | yes | 0 |
| 15 | `D:/knowledge/knowledge/ideas/G2-INSPIRED-SAAS-IDEAS.md` | yes | 0 |
| 16 | `D:/knowledge/knowledge/ops/TRUSTED-RESOURCES-GUIDE.md` | yes | 0 |
| 17 | `D:/knowledge/knowledge/ops/DISK-CLEANUP-PLAN.md` | yes | 0 |

Total markdown files in knowledge base: 17. Total consulted: 17. Total with
AutoSale-specific content: **0**.

Non-markdown / out-of-scope artifacts (listed for completeness, not consulted):
- `D:/knowledge/knowledge/.gitignore`
- `D:/knowledge/knowledge/.git/**` (internal git metadata)

---

## 16. Confidence Statement

The knowledge base was scanned exhaustively with multiple regex variants
(see section 1). The conclusion - that AutoSale is undocumented in the global
knowledge base - is **high confidence**. If a future agent finds AutoSale
documentation under `D:/knowledge/knowledge/`, it must have been added *after*
2026-05-08 (the date of this scan), and this document should be regenerated
against the new state of the knowledge base.
