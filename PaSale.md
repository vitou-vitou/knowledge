# PaSale - Complete Reference

> Branch: `3-integrate-travel`
> Workspace: `D:\g4vt\gpi-agency-portal`
> Knowledge base scanned: `D:\knowledge\knowledge` (42 files indexed, 18 non-`.git` content files read/searched)
> Generated: 2026-05-08
> Author: Cursor agent (subagent under parent run)

---

## 0. Executive Summary (READ ME FIRST)

**No PaSale references were found in the knowledge base at `D:\knowledge\knowledge`.**

After enumerating every file in the knowledge base and running case-insensitive
content + filename searches across the entire tree (see [§2 Search Methodology](#2-search-methodology)
and [§7 Sources](#7-sources)), there are zero matches for any of the PaSale-related
patterns we tried. The knowledge base is a **generic, cross-project reference**
covering multi-agent orchestration, model selection, Laravel/Filament/Livewire
stack notes, role-prompt playbooks, ops procedures, and SaaS ideas. It does not
contain any business-domain documentation for the `gpi-agency-portal` project,
its travel/agency workflows, or the PaSale (Passenger Sale / personal sale) flow.

> If you are a future agent who opened this file expecting a step-by-step PaSale
> workflow: it is not in the knowledge base. The authoritative source must live
> somewhere else — most likely inside this project's own `docs/`, the `app/`
> source tree (Filament resources, models, services), or an external G4VT/GPI
> business document that has not yet been imported into `D:\knowledge\knowledge`.
> See [§5 Where to Look Next](#5-where-to-look-next) for concrete next steps.

This document is therefore organized as:

1. What PaSale is — best-effort definition based on the term itself, since the
   knowledge base offers no definition.
2. The search methodology used to confirm absence.
3. A complete inventory of what the knowledge base **does** contain (so the
   next agent does not need to re-scan it).
4. A template/skeleton future agents can fill in once an authoritative PaSale
   source is located.
5. Concrete next steps to find the real PaSale documentation.
6. A glossary of related terms tried during the search.
7. A Sources section listing every file consulted with absolute paths.

---

## 1. What PaSale Is (Best-Effort Definition)

> The knowledge base contains no definition of PaSale. The following is an
> informed inference from the term and the surrounding project context (branch
> `3-integrate-travel`, repo `gpi-agency-portal`). Treat it as a placeholder to
> be replaced once an authoritative source is located.

- **PaSale** most likely abbreviates **"Passenger Sale"** (sometimes written
  "Pa Sale", "Pa-Sale", "Pax Sale", or "personal sale" in adjacent contexts).
- In a **travel agency portal** (`gpi-agency-portal`), a Passenger Sale is the
  end-to-end business process of selling a travel product (typically a flight
  ticket, sometimes bundled with hotel/insurance/visa) to an individual
  passenger or small party, as opposed to bulk/group/corporate sales.
- Likely actors: agency staff (sales/counter agent), supervisor/finance role,
  the end passenger (data subject, possibly not a system user), and one or
  more upstream travel suppliers (GDS, airline NDC, consolidator, hotel API).
- Likely artifacts: a sale/booking record, a passenger profile, an itinerary,
  a payment record, a commission record, a ticket/voucher, and audit trail
  entries for state transitions.

**This section is unverified.** Do not act on it. Locate the authoritative
spec (see [§5](#5-where-to-look-next)) before writing code.

---

## 2. Search Methodology

### 2.1 Files enumerated

`Glob` on `D:\knowledge\knowledge` returned 42 entries total. After excluding
`.git/**` plumbing (sample hooks, refs, logs, config), **18 content files**
remained:

```
D:\knowledge\knowledge\.gitignore
D:\knowledge\knowledge\AGENTS.md
D:\knowledge\knowledge\README.md
D:\knowledge\knowledge\ideas\G2-INSPIRED-SAAS-IDEAS.md
D:\knowledge\knowledge\ops\DISK-CLEANUP-PLAN.md
D:\knowledge\knowledge\ops\TRUSTED-RESOURCES-GUIDE.md
D:\knowledge\knowledge\playbooks\architect-template.md
D:\knowledge\knowledge\playbooks\bugfix-template.md
D:\knowledge\knowledge\playbooks\devops-template.md
D:\knowledge\knowledge\playbooks\feature-template.md
D:\knowledge\knowledge\playbooks\qa-template.md
D:\knowledge\knowledge\stacks\laravel\LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md
D:\knowledge\knowledge\stacks\laravel\LARAVEL-QUICK-LAUNCH.md
D:\knowledge\knowledge\strategies\HOW-TO-CONTINUE-CHATS.md
D:\knowledge\knowledge\strategies\MODELS-RANKING.md
D:\knowledge\knowledge\strategies\PARALLEL-AGENTS-ADVANCED.md
D:\knowledge\knowledge\strategies\SAAS-MULTI-AGENT-STRATEGY.md
D:\knowledge\knowledge\templates\agent-run\README.md
```

### 2.2 Patterns tried (case-insensitive, against full file contents)

| # | Pattern (regex / literal) | Files matched | Notes |
|---|---|---|---|
| 1 | `pasale\|pa[\s\-_]?sale\|passenger\s*sale\|pax\s*sale\|passenger\s*booking` | 0 | Combined PaSale variants. |
| 2 | `pasale` | 0 | Literal. |
| 3 | `passenger` | 0 | Standalone term. |
| 4 | `sale` | 1 | Only `ideas/G2-INSPIRED-SAAS-IDEAS.md`, and only as parts of "Salesforce" — unrelated to PaSale. |
| 5 | `pax\|booking\|travel\|airline\|flight\|ticket` | 0 | Adjacent travel-domain terms. |
| 6 | `pa.?sale\|pa_sale\|pasale\|p\.sale` | 0 | Punctuation variants. |
| 7 | `agency\|portal\|gpi\|g4vt\|integrate-travel\|travel` | 0 | Project-name and branch-name terms. |
| 8 | `\bPA\b\|\bPa\b` | 0 | Standalone "PA" / "Pa" tokens. |
| 9 | `PA` (substring, no boundary) | 11 | Only matches inside `<head>`-style tags or substrings like "**PA**RALLEL" / "**PA**RAGRAPH" / "T**PA**" — none refer to a Passenger entity. Reviewed and discarded. |

### 2.3 Filenames

No filename in the knowledge base contains `pasale`, `passenger`, `pax`,
`sale`, `booking`, `flight`, `ticket`, `travel`, `agency`, or `gpi`.

### 2.4 Confidence

High. The knowledge base is small (18 content files, ~all under 30 KB), each
file has a clear single topic stated in its title and `README.md` index, and
none of the topics intersect the travel-agency or passenger-sale domain.

---

## 3. What The Knowledge Base Actually Contains

Documenting this so the next agent does not need to re-scan. Source: the
authoritative index at `D:\knowledge\knowledge\README.md` plus direct reads.

> From `README.md`:
>
> > Single source of truth for cross-project methods, stacks, and playbooks.
> > Local: `D:\vitou\knowledge\` | Remote: <https://github.com/vitou-vitou/knowledge>

### 3.1 `strategies/` — cross-project methods

- `SAAS-MULTI-AGENT-STRATEGY.md` — the "5 specialist agents in parallel" core method.
- `PARALLEL-AGENTS-ADVANCED.md` — wave execution, hub-and-spoke, conflict resolution hierarchy.
- `MODELS-RANKING.md` — which Cursor model to pick for which job.
- `HOW-TO-CONTINUE-CHATS.md` — resuming long conversations without losing context.

### 3.2 `stacks/laravel/` — Laravel stack notes

- `LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md` — stack notes for Laravel + Filament + Livewire builds.
- `LARAVEL-QUICK-LAUNCH.md` — cold-start a Laravel project fast.

### 3.3 `playbooks/` — role-prompt templates

Forkable role specs with `[PROJECT]`, `[SCOPE]`, `[FILES]` placeholders:

- `architect-template.md`
- `bugfix-template.md`
- `devops-template.md`
- `feature-template.md`
- `qa-template.md`

### 3.4 `templates/agent-run/`

- `README.md` — drop-in scaffold for `docs/agents/` in a new project; pairs with the 5 playbooks.

### 3.5 `ideas/`

- `G2-INSPIRED-SAAS-IDEAS.md` — generic SaaS ideas inspired by G2 categories. Mentions "Salesforce" but no Passenger Sale.

### 3.6 `ops/`

- `DISK-CLEANUP-PLAN.md` — dev-machine disk hygiene.
- `TRUSTED-RESOURCES-GUIDE.md` — vetted external resources.

### 3.7 Top-level governance

- `AGENTS.md` — global agent rules (tone, hygiene, multi-agent run protocol). Project-specific `AGENTS.md` may extend or override.
- `README.md` — index and maintenance rules.

> Excerpt from `AGENTS.md` (Project Hygiene, lines 56-65):
>
> > - **Never commit** unless the user explicitly asks.
> > - **Never run** `composer install`, `npm install`, `php artisan migrate`, or `docker build` without explicit instruction.
> > - **Never edit** `.env` (live env). `.env.example` is fair game when scoped to a DevOps task.
> > - **Preserve typos in identifiers** (e.g. `TplClientControler.php` — single `l` — may be load-bearing). Confirm before renaming.
> > - **Stay on the user's current branch.** Don't switch branches autonomously.

These hygiene rules apply to any future agent implementing PaSale.

---

## 4. PaSale Reference Skeleton (To Be Filled)

Once the authoritative PaSale spec is located, copy this skeleton into the
top of this document and replace each `_TBD_` with the real content. The
structure mirrors the original task brief so nothing is missed.

```markdown
## A. Definition & Business Purpose
- What PaSale is in this project: _TBD_
- Why it exists / business outcome: _TBD_
- Who uses it (named roles): _TBD_
- In-scope vs out-of-scope: _TBD_

## B. End-to-End Workflow (every step, every sub-step)
1. _TBD_ (precondition / trigger)
2. _TBD_ (passenger lookup / create)
3. _TBD_ (product/itinerary search)
4. _TBD_ (quote / fare rules)
5. _TBD_ (hold / book with supplier)
6. _TBD_ (payment capture)
7. _TBD_ (issue ticket / voucher)
8. _TBD_ (post-issue: email, audit, commission)
9. _TBD_ (cancel / refund / reissue paths)

## C. Actors, Roles, Permissions
| Role | Filament panel(s) | Permissions | Notes |
|---|---|---|---|
| _TBD_ | _TBD_ | _TBD_ | _TBD_ |

## D. Data Model
- Tables / models involved: _TBD_
- Required vs optional fields: _TBD_
- Validation rules: _TBD_
- Foreign keys / relationships: _TBD_

## E. State Machine
- States: _TBD_
- Allowed transitions: _TBD_
- Terminal states: _TBD_
- Edge cases (timeout, supplier failure, partial payment): _TBD_

## F. Integration Points
- Filament resources & pages: _TBD_
- Livewire components: _TBD_
- Services / Actions / Jobs: _TBD_
- Events & Listeners: _TBD_
- HTTP / API endpoints: _TBD_
- Third parties (GDS / airline / payment): _TBD_

## G. Business Rules, Pricing, Commission, Compliance
- Pricing rules: _TBD_
- Commission split: _TBD_
- Tax / fee handling: _TBD_
- PCI / PII / data-retention: _TBD_
- Audit trail requirements: _TBD_

## H. UI / Screens / Commands
- Screens: _TBD_
- artisan / queue commands: _TBD_

## I. Related Playbooks, Ops Procedures, Templates
- (Reference each by absolute path.)
```

---

## 5. Where to Look Next

Concrete, prioritized search locations beyond the knowledge base. Each is an
absolute path the next agent can hand straight to `Glob` / `Grep` / `Read`.

1. **Project docs (highest signal):**
   - `D:\g4vt\gpi-agency-portal\docs\` — if it exists, this is where business
     workflows for this project live per the `AGENTS.md` rule
     "project-specific context still lives inside each project's own `docs/`".
   - `D:\g4vt\gpi-agency-portal\AGENTS.md` (if present) — project-level
     overrides may name the PaSale flow.
   - `D:\g4vt\gpi-agency-portal\README.md` — top-level orientation.

2. **Source tree (definitive but low-level):**
   - `D:\g4vt\gpi-agency-portal\app\Filament\` — search for `Pasale`, `PaSale`,
     `Passenger`, `Sale`, `Booking` resources / pages / widgets.
   - `D:\g4vt\gpi-agency-portal\app\Models\` — model classes will reveal the
     real entity names and relationships.
   - `D:\g4vt\gpi-agency-portal\app\Services\`, `app\Actions\`, `app\Jobs\`,
     `app\Events\` — the orchestration layer.
   - `D:\g4vt\gpi-agency-portal\database\migrations\` — schema is the ground truth.
   - `D:\g4vt\gpi-agency-portal\routes\` — route names often spell out the flow.
   - `D:\g4vt\gpi-agency-portal\app\Providers\Filament\LoginPanelProvider.php`
     — already modified on this branch per `git status`; may hint at panel
     scoping for PaSale.

3. **Branch context:**
   - The branch `3-integrate-travel` strongly implies a travel-supplier
     integration is in progress. PaSale is likely one of the consumers of that
     integration. Check open issues / PRs for issue #3.

4. **Recommended commands** (read-only; run from repo root):
   ```bash
   # Filename hits
   rg --files D:/g4vt/gpi-agency-portal | rg -i 'pasale|passenger|pax|sale|booking'
   # Content hits
   rg -i 'pasale|passenger\s*sale|pax\s*sale' D:/g4vt/gpi-agency-portal
   # Migration / schema hits
   rg -i 'create_(pasales|passenger|sales|bookings)' D:/g4vt/gpi-agency-portal/database
   ```

5. **If still nothing:** ask the human owner where the PaSale spec lives
   (Notion, Google Doc, internal wiki, paper SOP). Per `AGENTS.md` §5
   "When Stuck", prefer `AskQuestion` (multiple choice) over open-ended
   clarification.

---

## 6. Glossary of Terms Searched

| Term | Likely meaning | Found in KB? |
|---|---|---|
| PaSale | Passenger Sale (this project's term) | No |
| Pa Sale | spaced variant | No |
| Pa-Sale | hyphenated variant | No |
| Passenger Sale | full English | No |
| Pax Sale | aviation shorthand (`pax` = passengers) | No |
| Passenger Booking | adjacent concept | No |
| Booking | adjacent concept | No |
| Ticket / Flight / Airline / Travel | domain context | No |
| Agency / Portal / GPI / G4VT | project context | No |
| Sale (substring) | broad | 1 hit, only inside "Salesforce" — unrelated |

---

## 7. Sources

Every file consulted while producing this document, with absolute paths.
Files marked "indexed only" were enumerated by `Glob` but not opened
because their path or name made them clearly irrelevant (`.git/` plumbing).
Files marked "searched" were scanned by `Grep` across full content. Files
marked "read in full" were opened with `Read`.

### 7.1 Read in full

- `D:\knowledge\knowledge\README.md` — read in full.
- `D:\knowledge\knowledge\AGENTS.md` — read in full.

### 7.2 Searched (full content scanned by Grep, multiple patterns)

- `D:\knowledge\knowledge\.gitignore`
- `D:\knowledge\knowledge\ideas\G2-INSPIRED-SAAS-IDEAS.md` (only "sale" substring hit, inside "Salesforce" — confirmed unrelated)
- `D:\knowledge\knowledge\ops\DISK-CLEANUP-PLAN.md`
- `D:\knowledge\knowledge\ops\TRUSTED-RESOURCES-GUIDE.md`
- `D:\knowledge\knowledge\playbooks\architect-template.md`
- `D:\knowledge\knowledge\playbooks\bugfix-template.md`
- `D:\knowledge\knowledge\playbooks\devops-template.md`
- `D:\knowledge\knowledge\playbooks\feature-template.md`
- `D:\knowledge\knowledge\playbooks\qa-template.md`
- `D:\knowledge\knowledge\stacks\laravel\LARAVEL-FILAMENT-LIVEWIRE-STRATEGY.md`
- `D:\knowledge\knowledge\stacks\laravel\LARAVEL-QUICK-LAUNCH.md`
- `D:\knowledge\knowledge\strategies\HOW-TO-CONTINUE-CHATS.md`
- `D:\knowledge\knowledge\strategies\MODELS-RANKING.md`
- `D:\knowledge\knowledge\strategies\PARALLEL-AGENTS-ADVANCED.md`
- `D:\knowledge\knowledge\strategies\SAAS-MULTI-AGENT-STRATEGY.md`
- `D:\knowledge\knowledge\templates\agent-run\README.md`

### 7.3 Indexed only (Glob enumeration, not opened — `.git/` internals)

- `D:\knowledge\knowledge\.git\HEAD`
- `D:\knowledge\knowledge\.git\config`
- `D:\knowledge\knowledge\.git\description`
- `D:\knowledge\knowledge\.git\info\exclude`
- `D:\knowledge\knowledge\.git\packed-refs`
- `D:\knowledge\knowledge\.git\logs\HEAD`
- `D:\knowledge\knowledge\.git\logs\refs\heads\main`
- `D:\knowledge\knowledge\.git\logs\refs\remotes\origin\HEAD`
- `D:\knowledge\knowledge\.git\refs\heads\main`
- `D:\knowledge\knowledge\.git\refs\remotes\origin\HEAD`
- `D:\knowledge\knowledge\.git\hooks\applypatch-msg.sample`
- `D:\knowledge\knowledge\.git\hooks\commit-msg.sample`
- `D:\knowledge\knowledge\.git\hooks\fsmonitor-watchman.sample`
- `D:\knowledge\knowledge\.git\hooks\post-update.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-applypatch.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-commit.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-merge-commit.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-push.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-rebase.sample`
- `D:\knowledge\knowledge\.git\hooks\pre-receive.sample`
- `D:\knowledge\knowledge\.git\hooks\prepare-commit-msg.sample`
- `D:\knowledge\knowledge\.git\hooks\push-to-checkout.sample`
- `D:\knowledge\knowledge\.git\hooks\sendemail-validate.sample`
- `D:\knowledge\knowledge\.git\hooks\update.sample`

### 7.4 Counts

- Total files enumerated by `Glob`: **42**
- Content files searched: **18**
- Content files read in full: **2** (`README.md`, `AGENTS.md`)
- Files yielding any PaSale-related signal: **0**

---

## 8. Final Note for the Next Agent

If you are picking up this task: **do not re-scan `D:\knowledge\knowledge`**.
The knowledge base is generic and contains no domain content for this project.
Jump straight to [§5 Where to Look Next](#5-where-to-look-next) and start
inside `D:\g4vt\gpi-agency-portal\` (project docs, source, migrations). Once
you locate the authoritative PaSale spec, replace [§1](#1-what-pasale-is-best-effort-definition)
and fill in [§4](#4-pasale-reference-skeleton-to-be-filled).
