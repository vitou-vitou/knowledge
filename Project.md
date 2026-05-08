# gpi-agency-portal - Project Knowledge Reference

> Branch: features/auto
>
> Single self-contained onboarding document for any agent or contributor joining `gpi-agency-portal`. Synthesized from the global PGI knowledge base at `D:\knowledge\knowledge\` (mirror of `D:\vitou\knowledge\`) on 2026-05-08. If you read only one file before working here, read this one. Sibling deep-dives live in `D:\g4vt\gpi-agency-portal\3-integrate-travel\PaSale.md` and `D:\g4vt\gpi-agency-portal\features\auto\AutoSale.md`.

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [How to Use This Document](#2-how-to-use-this-document)
3. [Stack & Tooling](#3-stack--tooling)
4. [Repository Layout](#4-repository-layout)
5. [Domains & Modules](#5-domains--modules)
6. [Multi-Agent Workflow](#6-multi-agent-workflow)
   - 6.1 [Core 5-Agent Slate](#61-core-5-agent-slate)
   - 6.2 [Wave Execution](#62-wave-execution)
   - 6.3 [Conflict Resolution Hierarchy](#63-conflict-resolution-hierarchy)
   - 6.4 [Coordination Patterns](#64-coordination-patterns)
7. [Playbooks (Forkable Role Specs)](#7-playbooks-forkable-role-specs)
8. [Templates](#8-templates)
9. [Strategies](#9-strategies)
10. [Operations](#10-operations)
11. [Ideas / Roadmap](#11-ideas--roadmap)
12. [Conventions & Standards](#12-conventions--standards)
13. [Model Selection Cheat Sheet](#13-model-selection-cheat-sheet)
14. [Performance, Security, Monitoring](#14-performance-security-monitoring)
15. [Testing Strategy](#15-testing-strategy)
16. [Deployment Patterns](#16-deployment-patterns)
17. [Troubleshooting](#17-troubleshooting)
18. [Glossary](#18-glossary)
19. [Sources](#19-sources)

---

## 1. Project Overview

`gpi-agency-portal` (a.k.a. **PGI Agency Portal**) is a **Laravel + Filament + Livewire** monolith that powers PGI's agency-side operations. It is a multi-domain back-office portal whose responsibilities include:

- **Customer intake** (KYC-style profile capture, Cambodia-aware address handling).
- **Agency context** for staff who service clients (referrals, CIF, staff lookup).
- **Document management** (uploads, retention, audit).
- **PaSale (passenger / personal-account sale) journey** — the original sale lifecycle, currently being **integrated with Travel** systems (see `3-integrate-travel/PaSale.md`).
- **AutoSale** — auto-sale lifecycle (the parallel automation surface, see `features/auto/AutoSale.md`).
- **Entity context & lookups** — shared contracts for staff / referral / CIF / entity resolution.
- **Dev impersonation / user-profile impersonation** — inflight change to safely act-as another user during development and support.

The product is built and operated using PGI's **parallel multi-agent strategy** (see §6) — each "agent" is a Cursor / Claude / GPT specialist with a tightly scoped role (architect, backend, filament, livewire, devops, QA, bugfix, feature). The codebase, the docs (`docs/`, `openspec/`), and the agent workflow are co-designed: branches are typically named after the lane (`features/auto`, `3-integrate-travel`, etc.), and parallel agents write into non-overlapping scopes.

### High-Level Architecture

```
Browser
  │
  ▼
Filament Panels (admin)  ◄──────────── spatie/permission, filament-shield
Blade + Livewire (front-of-house)
  │
  ▼
Laravel 12 / PHP 8.2
  - app/Http/Controllers, app/Services, app/Filament, app/Livewire
  - Spatie Data DTOs (spatie/laravel-data ^4.19)
  - Filament Socialite + Keycloak (SSO)
  - QR code (chillerlan/php-qrcode, simplesoftwareio)
  - JWT (firebase/php-jwt) — Travel integration tokens
  │
  ▼
MySQL / PostgreSQL    Redis (sessions, cache, queues)
  │                       │
  └─── Eloquent ORM ──────┘
  │
  ▼
External integrations: Travel core APIs, Keycloak SSO, Cambodia address service
```

### Key Inflight & Archived OpenSpec Changes

The repo uses **OpenSpec** (`openspec/`) to track in-flight specifications and an archive of accepted changes. As of 2026-05-08:

| Path | Status |
|---|---|
| `openspec/specs/agency-portal/spec.md` | Accepted |
| `openspec/specs/customer-intake/spec.md` | Accepted |
| `openspec/specs/document-management/spec.md` | Accepted |
| `openspec/specs/entity-context/spec.md` | Accepted |
| `openspec/specs/entity-lookup-contracts/spec.md` | Accepted |
| `openspec/specs/staff-lookup/spec.md` | Accepted |
| `openspec/changes/user-profile-impersonation/` | In-flight |
| `openspec/changes/archive/2026-02-12-pgi-agency-portal/` | Archived (project genesis) |
| `openspec/changes/archive/2026-02-12-enhance-sale-journey/` | Archived (sale journey hardening) |
| `openspec/changes/archive/2026-02-16-entity-context-refactor/` | Archived (entity-context split) |

> When implementing, archiving, syncing, or verifying a change, always invoke the matching skill under `.claude/skills/openspec-*` or `.opencode/skills/openspec-*`. The corresponding command shortcut lives in `.claude/commands/opsx/`.

---

## 2. How to Use This Document

This file is the single ramp-up surface. The patterns below come directly from the global knowledge base at `D:\knowledge\knowledge\` (mirror of `D:\vitou\knowledge\`).

- **Building a feature?** Read §3 (Stack), §4 (Layout), §5 (Domains), §6 (Workflow), §7 (Feature playbook).
- **Debugging?** Read §6.3 (Conflict hierarchy), §7 (Bugfix playbook), §17 (Troubleshooting).
- **Operating prod?** Read §10 (Ops), §14 (Perf/Security), §16 (Deploy).
- **Onboarding to multi-agent runs?** Read §6 in full, then fork the right playbook from §7 into `docs/agents/`.
- **Anything cross-project?** The authoritative source remains `D:\knowledge\knowledge\` and its files cited in §19. This doc summarizes them but the master copies are versioned in git at `https://github.com/vitou-vitou/knowledge`.

> Sibling deep-dives:
> - `D:\g4vt\gpi-agency-portal\3-integrate-travel\PaSale.md` — full PaSale × Travel integration playbook.
> - `D:\g4vt\gpi-agency-portal\features\auto\AutoSale.md` — full AutoSale lifecycle.
>
> Do not duplicate their content here; cross-link instead.

---

## 3. Stack & Tooling

### Core Framework

| Layer | Choice | Notes |
|---|---|---|
| Language | **PHP ^8.2** | enforced in `composer.json` |
| Framework | **Laravel ^12.0** | `laravel/framework`, `laravel/tinker` ^2.10 |
| Admin | **Filament ^5.0** (`filament/filament`) | primary admin panel |
| Frontend interactivity | **Livewire 3.x** (transitively) | conflict pinned: `livewire/livewire` `<4.2.0` |
| Templating | **Blade** + Tailwind CSS + Alpine.js | per Laravel/Filament conventions |
| Data DTOs | **spatie/laravel-data ^4.19** | use Data classes for inbound payloads |
| SSO | **dutchcodingcompany/filament-socialite ^3.1**, **socialiteproviders/keycloak ^5.3** | Keycloak is the IdP |
| JWT | **firebase/php-jwt ^6.11** | Travel integration tokens |
| QR | **chillerlan/php-qrcode ^5.0**, **simplesoftwareio/simple-qrcode ^4.2** | dual stacks present — keep both until consolidated |
| Local geo | **pgi/cambodia-address @dev** (path repo at `packages/cambodia-address`) | first-party package |
| Dev | Laravel Sail ^1.41, Pint ^1.24, Pail ^1.2.2, Pest/PHPUnit ^11.5, Mockery ^1.6, Faker ^1.23 | |

### Composer Scripts (canonical commands)

```text
composer setup   # install + .env + key:generate + migrate + npm install + npm run build
composer dev     # concurrent: php artisan serve | queue:listen | pail | npm run dev
composer test    # config:clear + artisan test
```

> **Never run `composer setup`, `composer install`, `npm install`, `php artisan migrate`, or `docker build` autonomously** (see §12 — Project Hygiene).

### Recommended Additional Packages (per knowledge base)

The Laravel-Filament-Livewire strategy notes the following ecosystem packages as commonly useful:

- `spatie/laravel-permission` — role/permission management
- `spatie/laravel-activitylog` — audit trails
- `spatie/laravel-backup` — automated backups
- `barryvdh/laravel-debugbar` — dev debugging
- `nunomaduro/collision` — already present
- `filament/spatie-laravel-media-library-plugin` — file uploads
- `filament/spatie-laravel-settings-plugin` — application settings
- `filament/spatie-laravel-translatable-plugin` — multi-language
- `bezhansalleh/filament-shield` — permission UI

---

## 4. Repository Layout

Top-level folders (verified at HEAD on 2026-05-08):

```
gpi-agency-portal/
├── app/                      # Laravel app (Http, Models, Services, Filament, Livewire, Providers)
├── bootstrap/                # framework bootstrap
├── config/                   # Laravel config
├── database/                 # migrations, seeders, factories
├── docker/                   # Docker config (DevOps agent territory)
├── docs/                     # human + agent docs
│   ├── 3-integrate-travel/   # Travel integration notes (sibling to top-level branch folder)
│   └── openapi/              # OpenAPI specs (cif-, referral-, staff-lookup APIs)
├── features/                 # branch-named folders for parallel agent runs (THIS file lives here)
│   └── auto/                 # designated branch `features/auto`
├── 3-integrate-travel/       # designated branch folder (PaSale × Travel)
├── openspec/                 # OpenSpec changes & specs (in-flight + archive)
│   ├── changes/              # in-flight + archive
│   └── specs/                # accepted specs
├── packages/                 # path-repo first-party packages (cambodia-address)
├── public/                   # web root
├── resources/                # views (blade), js, css
├── routes/                   # web.php, api.php, console.php
├── skills/                   # project-local agent skills
├── storage/                  # logs, cache, framework state
├── tests/                    # PHPUnit/Pest suites
├── .claude/                  # Claude IDE config (commands, skills)
├── .opencode/                # OpenCode shortcuts
├── .idea/ / .editorconfig    # JetBrains + editor config
├── composer.json / .lock
├── package.json / lock
├── vite.config.js
├── phpunit.xml
├── artisan
└── README.md
```

Notable agent-workflow folders:

- `.claude/skills/openspec-*/SKILL.md` — every OpenSpec lifecycle step has a paired skill (apply, archive, bulk-archive, continue, explore, ff, new, onboard, sync-specs, verify-change).
- `.claude/commands/opsx/<verb>.md` — slash-command entry points that load the matching skill.
- `.opencode/` mirrors the same in OpenCode style.
- `skills/` and `skills-lock.json` — project-local registry of skills referenced by name.

---

## 5. Domains & Modules

The accepted OpenSpec specs (`openspec/specs/`) define the canonical domain set. For each, the source-of-truth spec lives in `openspec/specs/<domain>/spec.md`. **Read the spec before changing the domain.**

| # | Domain | Source Spec | Purpose |
|---|---|---|---|
| 1 | **agency-portal** | `openspec/specs/agency-portal/spec.md` | Top-level shell, navigation, panel composition, who-can-see-what for agency staff. |
| 2 | **customer-intake** | `openspec/specs/customer-intake/spec.md` | KYC-style intake — capture, validate, persist new customers. Cambodia-address aware via `pgi/cambodia-address`. |
| 3 | **document-management** | `openspec/specs/document-management/spec.md` | Upload/retain/audit docs against entities (customers, sales). |
| 4 | **entity-context** | `openspec/specs/entity-context/spec.md` | Runtime context (which entity is the user acting on). Refactored 2026-02-16 (see archive). |
| 5 | **entity-lookup-contracts** | `openspec/specs/entity-lookup-contracts/spec.md` | Shared contracts the lookup family must implement (CIF, referral, staff). |
| 6 | **staff-lookup** | `openspec/specs/staff-lookup/spec.md` | Staff identity lookup; OpenAPI at `docs/openapi/staff-lookup-api.md`. |

### Inflight Domain Work

| Path | Adds | Notes |
|---|---|---|
| `openspec/changes/user-profile-impersonation/` | `dev-impersonation` (new), modifies `agency-portal` and `user-profile` | "Act as another user" support for dev/support, gated. Read `proposal.md`, `design.md`, `tasks.md`, and the per-spec deltas before implementing. |

### Branch-Local Domain Lanes (parallel work)

| Branch / Folder | Topic | Deep-dive |
|---|---|---|
| `3-integrate-travel/` | PaSale × Travel integration | `D:\g4vt\gpi-agency-portal\3-integrate-travel\PaSale.md` |
| `features/auto/` | AutoSale automation surface | `D:\g4vt\gpi-agency-portal\features\auto\AutoSale.md` |

These lanes are written by parallel agents whose scopes do not overlap with each other or with this `Project.md`. Treat their `.md` outputs as authoritative for their lane.

### Lookup APIs (OpenAPI)

OpenAPI specs at `docs/openapi/`:

- `cif-lookup-api.md`
- `referral-lookup-api.md`
- `staff-lookup-api.md`

These define the request/response shapes for the entity-lookup family.

### Sale Journey Heritage

The archived change `openspec/changes/archive/2026-02-12-enhance-sale-journey/` consolidated the original sale flow with delta specs against `agency-portal`, `customer-intake`, `document-management`, and `staff-lookup`. Read `proposal.md` and `design.md` if you need the rationale for the current sale schema.

---

## 6. Multi-Agent Workflow

PGI's core method (verbatim from the SaaS multi-agent strategy):

> Instead of switching models manually or using 1 model sequentially, launch **5 specialist agents simultaneously** with different models and roles. Each agent works independently on their domain, then you merge the results.

### 6.1 Core 5-Agent Slate

From `D:\knowledge\knowledge\stacks\laravel\LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`:

| # | Role | Model | Owns |
|---|---|---|---|
| 1 | **Architect** | `claude-opus-4-7-thinking-xhigh` | DB schema, model relationships, Filament resource plan, Livewire wireframes, route arch |
| 2 | **Backend** | `gpt-5.5-medium` | controllers, services, form requests, jobs, seeders, factories |
| 3 | **Filament** | `claude-4-sonnet` | resources, pages, widgets, custom form components |
| 4 | **Livewire** | `composer-2-fast` | Livewire components, blade, layouts, real-time UX |
| 5 | **DevOps** | `gpt-5.3-codex` | Sail, Docker, CI/CD, env config, monitoring |

The generic 5-role slate from `D:\knowledge\knowledge\templates\agent-run\README.md` (used outside Laravel-specific runs):

| # | Role | Model | Source playbook |
|---|---|---|---|
| 01 | Bugfix | `gpt-5.3-codex-high-fast` | `bugfix-template.md` |
| 02 | Feature | `claude-4.6-sonnet-medium-thinking` | `feature-template.md` |
| 03 | DevOps | `gpt-5.5-medium` | `devops-template.md` |
| 04 | QA | `composer-2-fast` | `qa-template.md` |
| 05 | Architect (read-only) | `claude-opus-4-7-thinking-xhigh` | `architect-template.md` |

### 6.2 Wave Execution

Recommended sequencing:

```
Wave 1 — Foundation (10–15 min)
  Architect → migrations, model relationships, overall design

Wave 2 — Core development (30–45 min, parallel)
  Backend  → controllers/services/jobs/seeders
  Filament → resources/pages/widgets
  Livewire → components/blade

Wave 3 — Integration & polish (15–20 min)
  DevOps → Sail/Docker/CI
  All    → integration testing
```

> Total target: < 90 minutes for a feature, < 3 hours for a Laravel-stack SaaS-sized scope.

### 6.3 Conflict Resolution Hierarchy

When agent outputs collide:

1. **Architect** wins on system design / DB schema decisions.
2. **Backend** wins on data models / business logic / API shape.
3. **Filament** wins on admin UI patterns and resource organization.
4. **Livewire** wins on frontend component structure.
5. **DevOps** wins on deployment; adapts to application needs.
6. **QA** writes new tests against the final implementation; adapts last.

Common conflicts and the resolution:

| Conflict | Resolution |
|---|---|
| API mismatch | Backend wins, Frontend adapts |
| DB schema | Backend wins, Frontend queries adapt |
| Auth strategy | Architect wins, all others follow |
| Deployment method | DevOps wins, others provide requirements |

### 6.4 Coordination Patterns

From `PARALLEL-AGENTS-ADVANCED.md`:

- **Wave** (default, recommended) — Architect → parallel Backend+Filament+Livewire+DevOps → QA.
- **Hub-and-Spoke** (complex projects) — Architect is the hub; other agents check in periodically.
- **Conductor** — `claude-opus-4-7-thinking-xhigh` monitors all agents at 25/50/75%.

Cap parallelism at 5–6 agents:

> 5 agents = 10 possible conflicts. 10 agents = 45. 20 agents = 190.
>
> Beyond 6 agents, coordination overhead grows faster than parallelism benefits.

### Optimal Counts by Project Size

| Size | Agent Count | Waves |
|---|---|---|
| Small SaaS (most projects) | 5 | 1 wave or 3-wave Architect-first |
| Medium | 7–8 | 3 waves: Architect → Frontend+Backend+Mobile+DevOps → QA+Security+Docs |
| Large enterprise | 10–12 | 4 waves: Arch+DB → Frontend+Backend+Mobile → Infra+Security+Perf → Test+Docs+Deploy |

### Launching a Run (canonical procedure, from `templates/agent-run/README.md`)

1. Copy `templates/agent-run/` into `<project>/docs/agents/`.
2. For each role you need, copy `D:\knowledge\knowledge\playbooks\<role>-template.md` into `docs/agents/0X-<role>-<topic>.md`.
3. Replace every `[PLACEHOLDER]`. Verify scopes do not overlap (Architect/QA usually only write new docs/tests).
4. Launch all chosen agents in **a single message** with parallel `Task` calls and `run_in_background: true`. Use the model slug from each playbook header.
5. Wait for completion notifications. Review handoff docs in `docs/agents/handoff/`.

> "Launch all 5 agents in a single message" is non-negotiable for the parallel pattern to work.

---

## 7. Playbooks (Forkable Role Specs)

Source: `D:\knowledge\knowledge\playbooks\`. Each is a forkable Markdown spec with `[PLACEHOLDER]` slots. Below is a one-stop summary; **fork the file** rather than copying contents.

### 7.1 Bugfix — `playbooks/bugfix-template.md`

- **Model:** `gpt-5.3-codex-high-fast`
- **Scope:** primary controller + secondary controller/service + export/transform folder + view-folder snippets directly tied to the bug + (only if the bug) routes file.
- **Forbidden:** `.env*`, `config/**`, `composer.json`, `package.json`, migrations, `tests/`, other modules.
- **Mission summary:** end-to-end audit of the broken flow; fix query divergence between screen and export, missing filters on export, drifted column headers, memory blow-ups (chunk/`FromQuery`), type/format issues, empty-result-path exceptions.
- **DoD:** export matches screen 1:1 under the same filters; empty-result returns valid response; large dataset doesn't exhaust memory; handoff at `docs/agents/handoff/01-bugfix-handoff.md`.

> Verify by reading the corresponding view(s) and matching visible columns 1:1 to the export / transform output.

### 7.2 Feature — `playbooks/feature-template.md`

- **Model:** `claude-4.6-sonnet-medium-thinking`
- **Scope:** views folder, assets folder, **add** routes only, **append** new controller methods only.
- **Forbidden:** existing controller methods (Bugfix territory), export/transform code, `.env*`, `config/**`, package manifests, migrations, `tests/`.
- **Mission:** add `[FEATURE_NAME]` with N capabilities, UX polish (loading state, toast, keyboard shortcuts), strict input validation, single new route `[METHOD] [PATH]`.
- **No new package dependencies.** Match existing visual / JS conventions of the layout. Inspect before injecting.
- **DoD:** works after pagination/filter change; no console errors; no layout jitter; no lint errors in touched files; handoff at `docs/agents/handoff/02-feature-handoff.md`.

### 7.3 DevOps — `playbooks/devops-template.md`

- **Model:** `gpt-5.5-medium`
- **Scope:** `.env.example`, `config/[RELEVANT_CONFIG].php`, `docker/**`, `Dockerfile`, `Dockerfile.*`, `.dockerignore`, `render.yaml`/`fly.toml`/`vercel.json` (if present), handoff doc.
- **Forbidden:** `app/`, `Modules/`, `routes/`, `database/`, `tests/`, blade/JS, `composer.json`, `package.json`, live `.env`.
- **Mission:** deduplicate `.env.example` without dropping unique keys; group by section with `#--- Database ---` etc.; preserve `# Local fallback` blocks; quote values with `#`, `$`, spaces, backslashes; comment-only review of relevant config; safe Docker fixes only (missing `.env` ignore, wrong language version, missing PHP extensions like `oci8`/`pdo_oci`/`redis`/`gd`/`imagick`); risky changes proposed in handoff.
- **DoD:** zero duplicate keys (or every duplicate intentionally commented); required quoting applied; handoff lists every duplicate, every Docker issue, before/after line count.

### 7.4 QA — `playbooks/qa-template.md`

- **Model:** `composer-2-fast`
- **Scope:** `tests/**` (new files only), `[MODULE_PATH]/Tests/**` (new files only), handoff doc.
- **Forbidden:** existing test files, source under `app/`, module `Http/`/`Services/`/`Resources/`, `config/`, `routes/`, `.env*`, `composer.json`, `package.json`, `phpunit.xml`.
- **Mission:** write fast, isolated, deterministic feature tests for `[FEATURE_AREA]`. Layout suggested:
  - `[MODULE_PATH]/Tests/Feature/[Area]ListTest.php`
  - `[MODULE_PATH]/Tests/Feature/[Area]ExportTest.php`
  - `[MODULE_PATH]/Tests/Feature/[Area]BulkActionTest.php` (mark `@group pending-feature` if it lands later)
  - `tests/Feature/[Module]/HealthRouteTest.php`
- **Coverage:** happy path 200 vs 401/302; filter/search/date-range narrows; export endpoint headers + valid file structure; empty-result export 200 not 500; bulk action validates inputs.
- **Style:** use `RefreshDatabase` only if test DB is configured, else `markTestSkipped()`; read existing tests first to match auth-acting pattern; each test independent.
- **DoD:** new tests syntactically valid, namespaced, AAA structure, handoff at `docs/agents/handoff/04-qa-handoff.md`.

### 7.5 Architect — `playbooks/architect-template.md`

- **Model:** `claude-opus-4-7-thinking-xhigh`
- **Mode: read-only.** Writes only inside `docs/agents/architect/**`.
- **Scope:** `MODULE-MAP.md`, `DATA-FLOW.md`, `RISK-REGISTER.md`, `REFACTOR-BACKLOG.md`, `docs/agents/handoff/05-architect-handoff.md`.
- **Forbidden: any source code, any config, any route, any test, any `.env*`. Documentation only.**
- **Deliverables:**
  - **MODULE-MAP.md** — one-page map of `[PRIMARY_MODULE_PATH]`: subfolder purposes, per-controller 1-line summary + routes, per-service 1-line summary, mark dead code with `(?dead)`.
  - **DATA-FLOW.md** — ASCII flow for the core lifecycle: origin → service transforms → persistence (which tables / connections) → read-vs-write divergence → external integration touchpoints.
  - **RISK-REGISTER.md** — top 10 risks ordered by severity (`critical | high | medium | low`), 1-line description, file path, 1-line mitigation. Hunt for: hard-coded secrets, hard-coded internal IPs, missing HTTP timeouts/retries, missing rate limits on auth/OTP, N+1, mass-assignment risks, role checks in controllers vs middleware, single huge controllers, OS-specific paths from app code.
  - **REFACTOR-BACKLOG.md** — top 10 refactor candidates by ROI: what, why, effort `S|M|L`, risk `S|M|L`, suggested first commit.
  - **handoff** — index of the four docs + 3-bullet exec summary aimed at the team lead.
- **Constraints:** propose **incremental, reversible** refactors, not rewrites. Use domain-specific language — assume the reader knows it.
- **DoD:** all five docs ≤ 2 pages each; zero source files modified.

---

## 8. Templates

### 8.1 `templates/agent-run/`

Single forkable scaffold. Drop into `<project>/docs/agents/`, fork the role specs from `playbooks/`, fill placeholders, launch.

> The README explicitly enumerates the conflict-avoidance rules every agent must follow (see §12).

### 8.2 Per-Project Junction (optional)

If you want `@docs/global/...` references in chat:

```powershell
mklink /J docs\global D:\vitou\knowledge
```

The same applies for the local mirror at `D:\knowledge\knowledge`.

### 8.3 OpenSpec Skill Templates

The repo's `.claude/skills/openspec-*` and `.opencode/skills/openspec-*` are themselves a template family for the change-management lifecycle:

| Skill | Purpose |
|---|---|
| `openspec-onboard` | Guided onboarding through a complete workflow cycle. |
| `openspec-explore` | Thinking-partner mode for investigating a problem. |
| `openspec-new-change` | Start a new change with the experimental artifact workflow. |
| `openspec-continue-change` | Create the next artifact for an in-flight change. |
| `openspec-ff-change` | Fast-forward through artifact creation in one step. |
| `openspec-apply-change` | Implement tasks from a change. |
| `openspec-verify-change` | Validate implementation matches the artifact. |
| `openspec-archive-change` | Finalize and archive a completed change. |
| `openspec-bulk-archive-change` | Archive several parallel changes at once. |
| `openspec-sync-specs` | Sync delta specs into main specs without archiving. |

Slash-command shortcuts: `.claude/commands/opsx/{new,continue,ff,apply,verify,archive,bulk-archive,sync,explore,onboard}.md`.

---

## 9. Strategies

### 9.1 SaaS Multi-Agent Strategy (the cornerstone)

Source: `D:\knowledge\knowledge\strategies\SAAS-MULTI-AGENT-STRATEGY.md`. Headline:

> **Total time:** ~20–40 minutes (vs 2–4 hours sequential)
> **Quality:** Higher (each model plays to its strengths)
> **Effort:** 1 launch command, then wait

ROI table reproduced verbatim:

| Metric | Sequential | Parallel Multi-Agent | Improvement |
|---|---|---|---|
| Time to MVP | 8–20 hours | 1–2 hours | **10x faster** |
| Code quality | Variable | Specialized per domain | **Higher consistency** |
| Technical debt | Higher (rushed handoffs) | Lower (planned architecture) | **Better foundation** |
| Learning curve | Steep (context switching) | Gentle (focused outputs) | **Easier to follow** |
| Iteration speed | Slow (rebuild context) | Fast (modular changes) | **5x faster updates** |

**Don't use parallel for:** learning projects, highly experimental features, single-domain tasks, budget-constrained work.

### 9.2 Parallel Agents — Advanced

Source: `D:\knowledge\knowledge\strategies\PARALLEL-AGENTS-ADVANCED.md`. Highlights:

- **Wave execution** is the recommended default (see §6.2).
- **Hub-and-spoke** for complex projects.
- **Conductor pattern** for huge projects (Opus monitors at 25/50/75%).
- **Don't max parallel agents.** 5–6 is the sweet spot.
- **Strategic model use over max parallelism:** model rotation when an agent gets stuck, A/B testing for ambiguous tasks, domain-specific selection (Claude for planning, GPT for code generation, Composer for repetitive tasks).

Success metrics:

| Metric | Good | Needs improvement |
|---|---|---|
| Total time | < 2 hours | > 4 hours |
| Integration bugs | < 5 conflicts | > 15 conflicts |
| Agent conflicts | < 3 major | > 5 major |
| Rework | < 20% of code | > 50% of code |
| Launch readiness | MVP in 1 day | > 1 week polish |

### 9.3 Models Ranking

Source: `D:\knowledge\knowledge\strategies\MODELS-RANKING.md`. See §13 for the cheat sheet. Key recommendation:

- **Daily driver:** `claude-4.6-sonnet-medium-thinking`
- **Hardest problems:** `claude-opus-4-7-thinking-xhigh`
- **Speed work:** `composer-2-fast`
- **Auto mode on by default**, switch only when output is too shallow or too slow.

### 9.4 How to Continue Chats

Source: `D:\knowledge\knowledge\strategies\HOW-TO-CONTINUE-CHATS.md`. Hand-off pattern: each saved doc has a copy-paste-ready re-entry phrase, e.g.:

> "Continue my previous work. Read all files in C:\Users\PGI\Desktop\docs\ to understand the context, then help me proceed."

For this project, the same idea applies: future agents should be told "Read `D:\g4vt\gpi-agency-portal\features\auto\Project.md` first, then …".

### 9.5 Trusted Resources (when not to roll your own)

Source: `D:\knowledge\knowledge\ops\TRUSTED-RESOURCES-GUIDE.md`. Bottom line:

> Stop reinventing the wheel. Use battle-tested frameworks, boilerplates, and orchestration tools.

Top picks listed:

- **Multi-agent orchestration:** CrewAI (recommended for SaaS), LangGraph (cheapest, deterministic), AutoGen (Azure shops).
- **SaaS boilerplates:** Modern MERN (https://modernmern.com/), Gravity (https://www.usegravity.app/), supastarter (https://supastarter.dev/nextjs-multi-tenancy-template).
- **Cursor-native:** Cursor Worktrees (`.cursor/worktrees.json`), Cursor 3 Multi-Workspace.

> The hybrid recommendation: SaaS boilerplate + CrewAI/LangGraph orchestrating customization agents. This is **stack-agnostic guidance**; gpi-agency-portal is Laravel-based, so the parallel-agent pattern is what we adopt locally — see §6.

---

## 10. Operations

### 10.1 Disk-Cleanup Plan

Source: `D:\knowledge\knowledge\ops\DISK-CLEANUP-PLAN.md`. This is a **dev-machine** ops doc, not a production runbook, but it lives here because PGI dev machines tend to fill up (Docker hoards 60+ GB).

Phased approach:

| Phase | Target gain | Notes |
|---|---|---|
| 1 — Zero-risk wins | 7–10 GB | Recycle Bin, Temp, package-manager caches, Storage Sense, `cleanmgr /d C:` |
| 2 — Docker | 30–50 GB | `docker system prune -a -f`, `docker volume prune -f`, `docker builder prune -a -f`; aggressive WSL2 reclaim via `Optimize-VHD` |
| 3 — IDE / app caches | 10–15 GB | JetBrains caches/log/tmp, Cursor/VSCode logs+cache, Claude logs+cache (NEVER Local Storage / IndexedDB), Chrome/Brave cache, Postman/CapCut cache, `Dism.exe /online /Cleanup-Image /StartComponentCleanup` |
| 4 — User audit | 5–15 GB | Downloads > 100 MB, installer files, AppData\Programs unused tools |
| 5 — Optional | variable | `powercfg -h off`, shadow storage `vssadmin resize`, Windows.old, NTFS compression |

Verification snippet:

```powershell
powershell -NoProfile -Command "$v = Get-Volume -DriveLetter C; '{0:N2} GB free of {1:N2} GB ({2:N1}% used)' -f ($v.SizeRemaining/1GB), ($v.Size/1GB), ((1 - $v.SizeRemaining/$v.Size)*100)"
```

> **Long-term hygiene:** Storage Sense weekly, Docker disk image size limit, monthly `docker system prune -a -f`, Downloads under 5 GB, archive installers off-drive.

### 10.2 Production Ops (per Laravel-stack guidance)

From `LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`:

- **Performance:**
  - Indexes on frequently queried columns
  - Eager loading for relationships (avoid N+1 in Filament tables — define relationships in resource query)
  - Telescope to profile queries
  - Connection pooling
- **Caching:**
  - Redis for sessions and cache
  - `route:cache`, `config:cache`, `view:cache` in production
  - Lazy loading for Livewire components
  - Vite asset bundling, image optimization, CDN for static assets
- **Monitoring stack:**
  - **Telescope** (development)
  - **Horizon** (queue monitoring)
  - **Pulse** (application monitoring)
  - **Sentry** (error tracking)
- **Metrics to watch:** DB query performance, Livewire component load times, Filament page response times, queue job processing times.

### 10.3 Environments & Secrets

- `.env.example` is the spec. Never edit live `.env` (see §12).
- DevOps agent owns deduplication / quoting in `.env.example`.
- Group keys by section header (`#--- Database ---`, `#--- Mail ---`, etc.).
- Quote any value with `#`, `$`, spaces, or backslashes.
- Preserve tooling markers (e.g. `#<Refund>...#</Refund>`).
- When the same key appears twice with different values, keep the production / Linux / deployed value as active and move the other under `# Local fallback`. **Do not delete — comment.**

---

## 11. Ideas / Roadmap

Source: `D:\knowledge\knowledge\ideas\G2-INSPIRED-SAAS-IDEAS.md`. This is a forward-looking inspiration / research doc, not a backlog for `gpi-agency-portal` itself. Patterns to mine when proposing new domains:

- **Specialize within categories** (Salesforce): "X for remote teams" beats "generic X".
- **Simplify complex processes** (Gusto): hide complexity behind good UX.
- **Replace tool sprawl** (Notion): combine 2–3 tools' core features.
- **Community-driven growth** (Notion): templates / workflows shared by users.
- **Freemium + education** (HubSpot): give 70% away, monetize power users + teams.

Five reference templates exist in the source:

1. **Simple Invoice Generator** (Gusto-style simplicity, ~$2–5K MRR target)
2. **Team Task Tracker** (Asana structure + Notion simplicity, ~$3–8K MRR)
3. **Expense Tracker for Freelancers** (specialized vertical, ~$2–4K MRR)
4. **URL Shortener with Analytics** (HubSpot data approach, ~$5–15K MRR)
5. **Meeting Notes Assistant** (Notion all-in-one, ~$4–10K MRR)

> Pre-build validation checklist (paraphrased): 3+ competitors with clear pricing exist; 5+ customer interviews done; MVP buildable in 2–6 weeks; LTV > 3x CAC; distribution channel identified.

For `gpi-agency-portal`, the actionable roadmap items are tracked in `openspec/changes/` (currently `user-profile-impersonation`).

---

## 12. Conventions & Standards

### 12.1 Global Agent Rules (verbatim from `D:\knowledge\knowledge\AGENTS.md`)

> These rules apply to any Cursor agent working on a project under `D:\vitou\projects\`. Project-specific `AGENTS.md` may extend or override these rules.

#### Tone & Output

- Terse. High signal per token. No marketing language.
- No emojis unless the user explicitly asks.
- No filler comments in code (`// Increment counter`, etc.). Only comments that explain non-obvious intent or constraints.
- Cite existing code with `startLine:endLine:filepath` blocks. Cite proposed code with fenced blocks tagged with the language only.

#### Multi-Agent Runs

1. **Always write task specs as `.md` files first** under `docs\agents\` in the project, one per agent (`01-*.md` … `05-*.md`).
2. **Forks come from `D:\vitou\knowledge\playbooks\*-template.md`** — strip placeholders, fill in project specifics.
3. **Scopes must not overlap.** Each spec lists allowed files explicitly. Architect/QA typically only write new docs/tests; bugfix and feature touch source.
4. **Always include**: model slug, allowed files, forbidden actions (no composer/npm/artisan/commit), handoff doc path.
5. **Launch all agents in a single message** with parallel Task calls. `run_in_background: true`.
6. **Use available model slugs only.** If the user picks one not in the available list, tell them which is unavailable rather than silently substituting.

#### Project Hygiene (read carefully — these are the hard rules for this repo)

- **Never commit** unless the user explicitly asks.
- **Never run** `composer install`, `npm install`, `php artisan migrate`, or `docker build` without explicit instruction.
- **Never edit** `.env` (live env). `.env.example` is fair game when scoped to a DevOps task.
- **Preserve typos in identifiers** (e.g. `TplClientControler.php` — single `l` — may be load-bearing). Confirm before renaming.
- **Stay on the user's current branch.** Don't switch branches autonomously.

#### When Stuck

- Don't loop. After two failed attempts at the same fix, write a short `STUCK.md` note describing what you tried, what failed, and what you'd try next, then ask the user.
- Prefer `AskQuestion` (multiple choice) over open-ended clarification when a decision has < 5 plausible answers.

### 12.2 Conflict-Avoidance Rules (every agent in a parallel run)

From `templates/agent-run/README.md`:

1. **Stay within your scope.** If you need to change a file outside your scope, write a note in `docs/agents/handoff/<your-id>-handoff.md` instead of editing.
2. **Do not touch** `composer.json`, `composer.lock`, `package.json`, `package-lock.json` unless your task explicitly requires it.
3. **Do not run** `composer install`, `npm install`, `php artisan migrate`, or `docker build`.
4. **Commit nothing.** Leave changes in the working tree for the user to review.
5. **Preserve existing typos in identifiers** (e.g. legacy file names) unless the user confirms a rename.

### 12.3 Laravel-Specific Best Practices (synthesized)

- **Database design:** migrations for all schema changes; relationships defined clearly in models; soft deletes where appropriate; factories for testing data.
- **Filament integration:** one resource per model as baseline; custom pages for complex workflows; widgets for dashboard insights; custom fields for specific needs.
- **Livewire patterns:** single-responsibility per component; `wire:model` for two-way binding; loading states for UX; events for component communication.
- **Performance:** eager loading to prevent N+1; caching for hot data; queue long-running tasks; optimize Livewire component rendering.
- **Multi-tenancy (when needed):** Architect defines tenant isolation strategy; Backend implements tenant scoping; Filament adds tenant context switching; Livewire respects tenant boundaries.

### 12.4 Maintenance Rules (knowledge base itself)

From `D:\knowledge\knowledge\README.md`:

- **Generic only.** No project-specific names, IPs, secrets, or schema details belong in the knowledge base. Project-specific docs stay inside the project's `docs/` (or here under `features/auto/`, `3-integrate-travel/`).
- **Edit in place.** The knowledge base is the single source of truth. Don't duplicate into project folders — junction or `@`-reference.
- **Date your changes.** Add `> Updated: YYYY-MM-DD` at the top of any doc you edit.
- **Generalize aggressively.** When a one-off project doc proves useful twice, strip the specifics and promote it into `playbooks/` or `stacks/`.

---

## 13. Model Selection Cheat Sheet

Source: `D:\knowledge\knowledge\strategies\MODELS-RANKING.md`.

| Rank | Model | Tier | Best For | Speed | Reasoning | Cost |
|:---:|---|---|---|:---:|:---:|:---:|
| 1 | `claude-opus-4-7-thinking-xhigh` | Flagship | hardest debugging, architecture, deep refactors, ambiguous specs | Slow | 10/10 | Highest |
| 2 | `claude-4.6-sonnet-medium-thinking` | Pro | daily coding, careful edits, multi-file refactors, planning | Medium | 9/10 | High |
| 3 | `gpt-5.5-medium` | Pro | general reasoning, mixed coding + writing, broad knowledge | Medium | 8.5/10 | High |
| 4 | `gpt-5.3-codex` (a.k.a. `gpt-5.3-codex-high-fast`) | Specialist | pure coding, scripting, repetitive edits | Medium-Fast | 8/10 | Medium |
| 5 | `composer-2-fast` | Fast | quick edits, simple commands, low-stakes tasks | Very Fast | 6.5/10 | Low |

Quick selection:

| Task | Pick |
|---|---|
| Weird bug nobody can find | 1. Opus xhigh |
| Refactor 10 files | 1. Opus or 2. Sonnet |
| Build a new feature end-to-end | 2. Sonnet |
| Plan / discuss / review architecture | 2. Sonnet (or 1. Opus for huge systems) |
| Mixed code + writing + research | 3. GPT 5.5 medium |
| Generate routine code | 4. GPT 5.3 codex |
| Rename, format, small tweak | 5. Composer 2 fast |
| PC cleanup / simple shell | 5. Composer 2 fast or 3. GPT 5.5 |
| Auto mode is on, what do I do? | Leave Auto on. Switch only when output feels too shallow or too slow. |

---

## 14. Performance, Security, Monitoring

### 14.1 Performance Optimization Checklist

From `LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`:

**Database:**
- [ ] Indexes on frequently queried columns
- [ ] Eager loading for relationships
- [ ] Query optimization via Laravel Telescope
- [ ] DB connection pooling

**Caching:**
- [ ] Redis for sessions and cache
- [ ] Route caching in production
- [ ] Config caching in production
- [ ] View caching for Blade templates
- [ ] Livewire asset caching

**Frontend:**
- [ ] Lazy loading for Livewire components
- [ ] Asset bundling with Vite
- [ ] Image optimization for uploads
- [ ] CDN integration for static assets

### 14.2 Security

**Laravel built-ins (assumed on):**
- CSRF protection (default)
- SQL injection prevention (Eloquent ORM)
- XSS protection (Blade escaping)
- Mass-assignment protection (`fillable`/`guarded`)

**Filament:**
- Authentication middleware on all admin routes
- Permission-based navigation hiding
- Resource-level authorization policies
- Audit logging for admin actions

**Livewire:**
- Component authorization in `mount`/`render`
- Input validation on all public properties
- Rate limiting on component actions
- CSRF on form submissions

**Architect's standing checklist** (from `architect-template.md`):
- Hard-coded secrets
- Hard-coded internal IPs
- Missing HTTP timeouts/retries
- Missing rate limits on auth/OTP
- N+1 queries
- Mass-assignment risks
- Role checks in controllers vs middleware
- Single huge controllers
- OS-specific paths from app code

### 14.3 Monitoring

```
Telescope  — dev profiling
Horizon    — queue monitoring
Pulse      — app monitoring
Sentry     — error tracking
```

Watch metrics: DB query perf, Livewire component load times, Filament page response times, queue job processing times.

---

## 15. Testing Strategy

### 15.1 Tooling

- **PHPUnit ^11.5** (active suite, see `phpunit.xml`)
- **Mockery ^1.6** for unit-test doubles
- **fakerphp/faker ^1.23** for fixtures
- **Pest** is allowed via `pestphp/pest-plugin` allow-plugin entry but the active suite is PHPUnit.

### 15.2 Where Tests Live

- `tests/` — top-level Laravel test suites.
- `[MODULE_PATH]/Tests/Feature/` — when modules/Filament resources isolate their own tests.

### 15.3 What to Cover (per `qa-template.md`)

- Happy path: list/show 200 for authenticated user, 401/302 for unauth.
- Filter / search / date-range narrows results correctly.
- Export / report endpoint returns correct headers + valid file structure.
- Empty-result export returns 200 with valid headers, **not 500**.
- Bulk action endpoint validates inputs and rejects invalid actions.

### 15.4 Style

- Use `RefreshDatabase` only if a test DB is configured; otherwise `markTestSkipped()` so the suite never blows up on a dev box.
- **Read existing tests under `tests/` first** to match the project's auth-acting pattern (Sanctum / Passport / standard guard).
- Each test independent. No reliance on test ordering.
- Backend agent: feature tests for controllers; unit tests for services; DB tests for models; API tests for endpoints.
- Livewire agent: component interaction tests; form validation tests; real-time feature tests; browser tests with Laravel Dusk.
- Filament agent: resource CRUD tests; custom page tests; widget functionality tests; permission integration tests.

---

## 16. Deployment Patterns

### 16.1 Laravel Forge

```yaml
git pull origin main
composer install --no-dev --optimize-autoloader
php artisan migrate --force
php artisan config:cache
php artisan route:cache
php artisan view:cache
npm run build
```

### 16.2 Laravel Vapor (serverless)

```yaml
environments:
  production:
    memory: 1024
    runtime: php-8.2
    database: laravel-app
    cache: laravel-cache
```

### 16.3 Docker (production)

```dockerfile
FROM php:8.2-fpm-alpine AS base
FROM node:18-alpine AS assets
FROM base AS production
```

### 16.4 Local Dev (Sail)

```bash
./vendor/bin/sail up
```

Success checklist after a multi-agent run:

- [ ] `php artisan migrate` runs cleanly
- [ ] Filament loads at `/admin` without errors
- [ ] Frontend renders main pages
- [ ] Auth (login/register) works
- [ ] `./vendor/bin/sail up` starts the dev environment

---

## 17. Troubleshooting

### 17.1 Common Laravel-Filament-Livewire Pitfalls

| Problem | Fix |
|---|---|
| Migration conflicts | Architect defines schema first, others follow. |
| Filament resource clashes with custom logic | Backend exposes proper model methods; resource calls them. |
| Livewire performance — too many DB queries | Eager loading + caching. |
| Different local setups | Standardize on Laravel Sail. |
| Filament conflicts with custom Livewire components | Use Filament's Livewire component base classes. |
| Authentication middleware conflicts | Configure separate guards for admin vs frontend. |
| Asset compilation conflict (Filament vs custom CSS) | Separate Vite configurations. |
| Slow Livewire rendering | Lazy loading + component caching. |
| N+1 in Filament tables | Define relationships in resource query (`getEloquentQuery()`). |
| Large file uploads timing out | Configure chunked uploads with Livewire. |

### 17.2 Multi-Agent Run Failure Modes

| Symptom | Fix |
|---|---|
| Agents create incompatible code | Run Architect first; include exact specs in each prompt; apply conflict-resolution hierarchy. |
| Agents finish at different times | `run_in_background: true`; integrate as agents complete. |
| One agent is stuck | Resume that agent with clarification; or restart with a different model; continue with the rest. |
| Integration takes forever | Design integration points upfront; standardize interfaces (REST contracts, DB schemas); test integration incrementally. |
| Model is disabled | Settings → Models → enable. Most common causes: regional restrictions, backend auto-disable, manual disable, provider outage, subscription tier. |

### 17.3 When You're Stuck (per AGENTS.md §5)

- Don't loop. After two failed attempts at the same fix, write `STUCK.md` describing what you tried, what failed, and what you'd try next, then ask the user.
- Prefer multiple-choice clarifications over open-ended ones when there are < 5 plausible answers.

---

## 18. Glossary

| Term | Meaning |
|---|---|
| **PGI** | Parent organization. Owns the agency portal and the cross-project knowledge base. |
| **Agency portal** | Top-level shell domain — staff-facing UI for agency operations. |
| **PaSale** | Personal/passenger account sale lifecycle in the portal. Currently being integrated with Travel — see `3-integrate-travel/PaSale.md`. |
| **AutoSale** | Auto-sale lifecycle / automation surface — see `features/auto/AutoSale.md`. |
| **CIF** | Customer Information File — canonical customer identity. Lookup API at `docs/openapi/cif-lookup-api.md`. |
| **Referral** | Channel-partner / introducer entity, tied to the staff-lookup family. |
| **Staff lookup** | Identity-resolution endpoint family for internal staff. |
| **Entity context** | Runtime "who am I acting on" abstraction shared across domains. |
| **Entity-lookup contracts** | Shared interfaces every lookup adapter (CIF, referral, staff) must implement. |
| **Customer intake** | New-customer KYC capture, Cambodia-address aware. |
| **Document management** | Upload/retain/audit attached docs. |
| **Dev impersonation / user-profile impersonation** | "Act as another user" capability — currently in-flight in `openspec/changes/user-profile-impersonation/`. |
| **OpenSpec** | The change-management workflow under `openspec/`: each change has `proposal.md`, `design.md`, `tasks.md`, and per-spec deltas; archive once accepted. |
| **opsx** | Slash-command namespace (`.claude/commands/opsx/`) that wraps the OpenSpec skills. |
| **Filament** | Laravel admin-panel framework powering the back-office UI (`filament/filament ^5.0`). |
| **Livewire** | Server-rendered reactive component framework. Pinned `< 4.2.0` by composer conflict. |
| **Sail** | `laravel/sail` — Docker-based local dev environment. |
| **Pest / PHPUnit** | Test runners. Active suite is PHPUnit ^11.5; Pest plugin is allowed. |
| **Pint** | Laravel's opinionated formatter. |
| **Pail** | `laravel/pail` — log streamer used in `composer dev`. |
| **Keycloak** | SSO IdP, wired via `socialiteproviders/keycloak` + `dutchcodingcompany/filament-socialite`. |
| **JWT** | Used by `firebase/php-jwt` for Travel integration tokens. |
| **Cambodia-address** | First-party `pgi/cambodia-address` package at `packages/cambodia-address`. |
| **DTO (Spatie Data)** | `spatie/laravel-data` Data classes used for typed request/response bags. |
| **Wave** | Phase in the parallel-agent execution model (Wave 1 = Architect; Wave 2 = parallel core; Wave 3 = integration). |
| **Hub-and-Spoke** | Coordination pattern where Architect remains an ongoing hub for the run. |
| **Conductor** | Coordination pattern where an Opus agent monitors the run at 25/50/75%. |
| **Handoff doc** | `docs/agents/handoff/0X-<role>-handoff.md` — required output of every agent role. |
| **STUCK.md** | Convention from AGENTS.md — write this rather than loop. |
| **Architect (read-only)** | The architect role only writes docs; never source. Enforced. |
| **Trusted resource** | A battle-tested external tool (CrewAI, LangGraph, Modern MERN, etc.) used in lieu of custom infra. See §9.5. |

---

## 19. Sources

Every file under `D:\knowledge\knowledge\` (excluding `.git/` internals and `.gitignore` meta) was consulted in producing this document.

| # | Absolute Path | One-line Summary |
|---|---|---|
| 1 | `D:\knowledge\knowledge\AGENTS.md` | Global Cursor-agent rules (knowledge base, tone, multi-agent, hygiene, when-stuck). |
| 2 | `D:\knowledge\knowledge\README.md` | Index of the knowledge base; folder taxonomy; maintenance rules. |
| 3 | `D:\knowledge\knowledge\strategies\SAAS-MULTI-AGENT-STRATEGY.md` | The "5 specialist agents in parallel" core method, ROI analysis, sample launch message. |
| 4 | `D:\knowledge\knowledge\strategies\PARALLEL-AGENTS-ADVANCED.md` | Wave / hub-and-spoke / conductor coordination, conflict resolution hierarchy, success metrics, troubleshooting. |
| 5 | `D:\knowledge\knowledge\strategies\MODELS-RANKING.md` | Master model ranking table + selection cheat sheet + speed-vs-intelligence map. |
| 6 | `D:\knowledge\knowledge\strategies\HOW-TO-CONTINUE-CHATS.md` | Copy-paste re-entry phrases for resuming saved-doc chats; per-file model recommendations. |
| 7 | `D:\knowledge\knowledge\stacks\laravel\LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md` | Full Laravel-stack 5-agent strategy: roles, packages, perf, security, monitoring, testing, deployment, troubleshooting. |
| 8 | `D:\knowledge\knowledge\stacks\laravel\LARAVEL-QUICK-LAUNCH.md` | One-message multi-agent deploy template + 5 quick project archetypes + 15-min success checklist. |
| 9 | `D:\knowledge\knowledge\playbooks\architect-template.md` | Read-only architect playbook: MODULE-MAP, DATA-FLOW, RISK-REGISTER, REFACTOR-BACKLOG, handoff. |
| 10 | `D:\knowledge\knowledge\playbooks\bugfix-template.md` | Bugfix playbook: end-to-end audit + fix scope; query divergence, exports, memory, type/format, empty-result. |
| 11 | `D:\knowledge\knowledge\playbooks\feature-template.md` | Feature playbook: append-only routes/methods, UX polish list, no new deps. |
| 12 | `D:\knowledge\knowledge\playbooks\devops-template.md` | DevOps playbook: dedupe `.env.example`, comment-only config, safe Docker fixes, propose risky changes. |
| 13 | `D:\knowledge\knowledge\playbooks\qa-template.md` | QA playbook: new test files only; suggested file layout; required coverage; auth-acting style. |
| 14 | `D:\knowledge\knowledge\templates\agent-run\README.md` | Forkable `docs/agents/` scaffold + default 5-role slate + conflict-avoidance rules. |
| 15 | `D:\knowledge\knowledge\ops\TRUSTED-RESOURCES-GUIDE.md` | Why use battle-tested orchestration frameworks and SaaS boilerplates instead of rolling your own. |
| 16 | `D:\knowledge\knowledge\ops\DISK-CLEANUP-PLAN.md` | 5-phase Windows dev-machine cleanup runbook (zero-risk → Docker → IDE caches → user audit → optional). |
| 17 | `D:\knowledge\knowledge\ideas\G2-INSPIRED-SAAS-IDEAS.md` | G2 winners' patterns + 5 SaaS project templates with multi-agent launch messages. |

> Sibling deep-dives (written concurrently by parallel agents — referenced, not duplicated):
> - `D:\g4vt\gpi-agency-portal\3-integrate-travel\PaSale.md`
> - `D:\g4vt\gpi-agency-portal\features\auto\AutoSale.md`
>
> Repository project files consulted for grounding (not part of the knowledge base):
> - `D:\g4vt\gpi-agency-portal\composer.json` (stack & package versions)
> - `D:\g4vt\gpi-agency-portal\README.md` (default Laravel readme)
> - `D:\g4vt\gpi-agency-portal\openspec\specs\*\spec.md` (accepted domain specs)
> - `D:\g4vt\gpi-agency-portal\openspec\changes\` (in-flight + archived changes)
> - `D:\g4vt\gpi-agency-portal\docs\openapi\` (CIF/referral/staff lookup APIs)

---

*End of `Project.md`. Future agents: if anything in §5 (Domains) drifts from `openspec/specs/`, the spec wins. If anything in §6 / §7 / §12 drifts from `D:\knowledge\knowledge\`, the knowledge base wins. Update this file in place rather than forking.*
