# Advanced Parallel Agent Techniques - Chat Notes

> Extended discussion on parallel agent coordination, model availability, and optimization strategies.
> Date: 2026-05-07 | Follow-up to SAAS-MULTI-AGENT-STRATEGY.md

---

## Key Discussion Points

### 1. Model Availability Issues

**Why Some Models Are Disabled:**
- Regional restrictions (provider policy, not Cursor)
- Backend changes auto-disable models (need manual re-enable)
- User settings (manually disabled for clean UI)
- Provider issues (OpenAI, Anthropic, Google outages)
- Subscription tier requirements

**Fix:** Settings → Models → Check/enable desired models

---

### 2. Why NOT Max Parallel Agents (20-30)?

**Coordination Complexity:**
- 5 agents = 10 possible conflicts
- 10 agents = 45 possible conflicts  
- 20 agents = 190 possible conflicts

**Diminishing Returns:**
| Agents | Value | Issues |
|:---:|---|---|
| 1-3 | High value | Minimal conflicts |
| 4-6 | Good value | Manageable coordination |
| 7-10 | Medium value | Coordination overhead |
| 11+ | **Negative value** | More coordination than coding |

**Resource Constraints:**
- Token cost: 20 agents = 20x consumption vs 5 agents
- Attention limits: Can't monitor 20 outputs effectively
- Integration bottleneck: Sequential regardless of parallel agents

---

### 3. Advanced Coordination Patterns

#### Wave Execution (Recommended)
```
Wave 1: Architect (claude-opus-4-7-thinking-xhigh)
Wave 2: Frontend + Backend + DevOps (parallel)
Wave 3: QA + Integration (composer-2-fast)
```

#### Hub-and-Spoke (Complex Projects)
```
Hub: Architect (ongoing coordination)
Spokes: Frontend, Backend, DevOps (check in with Hub)
Integration: QA pulls everything together
```

#### Conductor Pattern
```
Conductor: claude-opus-4-7-thinking-xhigh (monitors all agents)
Other agents: Check in at 25%, 50%, 75% completion
```

---

### 4. Conflict Resolution Hierarchy

| Priority | Agent | Reasoning |
|:---:|---|---|
| 1 | Architect | System design decisions are foundational |
| 2 | Backend | Data models affect everything else |
| 3 | Frontend | UI/UX decisions impact user experience |
| 4 | DevOps | Infrastructure adapts to application needs |
| 5 | QA | Tests adapt to final implementation |

**Common Conflicts:**
- API mismatch → Backend wins, Frontend adapts
- Database schema → Backend wins, Frontend queries adapt
- Auth strategy → Architect wins, all others follow
- Deployment method → DevOps wins, others provide requirements

---

### 5. Real Execution Example: Invoice Generator

**Step 1: Architect First (15 min)**
```
Task 1 (claude-opus-4-7-thinking-xhigh):
"Design complete system architecture for invoice generator SaaS:
- Database schema (users, clients, invoices, line_items)
- API contract (REST endpoints with request/response)
- Authentication strategy (JWT vs sessions)
- File structure for React + Node.js
- Integration points between frontend/backend
Deliver: ARCHITECTURE.md with complete specifications"
```

**Step 2: Wait for Architecture, Then Launch Wave 2 (30-40 min)**
```
Task 2 (claude-4.6-sonnet-medium-thinking): React frontend
Task 3 (gpt-5.3-codex): Node.js backend  
Task 4 (gpt-5.5-medium): Deployment infrastructure
All follow ARCHITECTURE.md specifications
```

**Step 3: Integration & QA (10-15 min)**
```
Task 5 (composer-2-fast): Integration and testing
```

---

### 6. When NOT to Use Parallel Agents

**Don't Use Parallel For:**
- Learning projects (want to understand each step)
- Highly experimental features (too much unknown)
- Single-domain tasks (all frontend, all backend, etc.)
- Budget constraints (parallel uses more tokens)

**Use Sequential Instead When:**
- Building your first SaaS (learn the process)
- Prototyping/validating idea quickly
- Working with unfamiliar technology
- Project has <3 distinct domains

---

### 7. Optimal Agent Counts by Project Size

#### Small SaaS (Most Projects)
**5 agents max:**
1. Architect
2. Frontend  
3. Backend
4. DevOps
5. QA

#### Medium Projects  
**7-8 agents in 3 waves:**
- Wave 1: Architect
- Wave 2: Frontend + Backend + Mobile + DevOps
- Wave 3: QA + Security + Docs

#### Large Enterprise Projects
**10-12 agents in 4 waves:**
- Wave 1: Architecture + Database design
- Wave 2: Core development (Frontend + Backend + Mobile)
- Wave 3: Infrastructure + Security + Performance
- Wave 4: Testing + Documentation + Deployment

---

### 8. Success Metrics

| Metric | Good | Needs Improvement |
|---|---|---|
| Total Time | <2 hours | >4 hours |
| Integration Bugs | <5 conflicts | >15 conflicts |
| Agent Conflicts | <3 major | >5 major |
| Rework Required | <20% of code | >50% of code |
| Launch Readiness | MVP ready in 1 day | Needs >1 week polish |

---

### 9. Troubleshooting Common Issues

#### Agents Create Incompatible Code
**Solution:** 
- Always run Architect first
- Include exact specs in each agent's prompt
- Use conflict resolution hierarchy

#### Agents Finish at Different Times
**Solution:**
- Use `run_in_background: true` for all agents
- Monitor progress, don't wait for all to finish
- Start integration as soon as 2+ agents complete

#### One Agent Gets Stuck
**Solution:**
- Resume that specific agent with clarification
- Or restart with different model
- Continue with other agents, integrate stuck one later

#### Integration Takes Forever
**Solution:**
- Design integration points upfront (Architect role)
- Use standardized interfaces (REST APIs, database contracts)
- Test integration incrementally, not all at once

---

### 10. Key Insight: The Trade-off Formula

```
Efficiency = (Agent_Speed × Parallelism) - (Coordination_Overhead)

Sweet Spot: 5-6 agents
- High parallelism benefit  
- Manageable coordination overhead
- Clear domain boundaries
```

**Beyond 6 agents:** Coordination overhead grows faster than parallelism benefits.

---

## Strategic Use of 20-30 Available Models

Instead of maxing parallel agents, use multiple models **strategically:**

### Model Rotation
- If an agent gets stuck, restart with different model
- Frontend struggling? Switch from Sonnet to Opus
- Backend too slow? Switch from GPT to Codex

### A/B Testing  
- Run same agent role with 2 different models
- Compare outputs, pick the better approach
- Learn which models work best for specific tasks

### Specialized Tasks
- **Claude models:** Better for planning, architecture, complex logic
- **GPT models:** Better for code generation, APIs, standard patterns  
- **Composer:** Better for repetitive tasks, testing, documentation

### Domain-Specific Selection
- **Writing-heavy tasks:** Claude Sonnet/Opus
- **Pure coding tasks:** GPT Codex
- **Infrastructure/DevOps:** GPT 5.5 (broad knowledge)
- **Speed-critical tasks:** Composer Fast

---

## Next Steps

With this advanced knowledge:

1. **Start with 5-agent projects** to master coordination
2. **Experiment with 6-7 agents** on larger projects  
3. **Use model variety strategically** rather than max parallelism
4. **Measure success metrics** to improve your process
5. **Build templates** for common coordination patterns

**The goal isn't maximum parallelism—it's maximum efficiency with manageable complexity.**

---

## Related Files in This Collection

- **SAAS-MULTI-AGENT-STRATEGY.md** - Core parallel strategy
- **MODELS-RANKING.md** - Model selection guide
- **G2-INSPIRED-SAAS-IDEAS.md** - Ready-to-build templates
- **DISK-CLEANUP-PLAN.md** - Example of detailed execution plan

---

Prepared: 2026-05-07
This document: Advanced parallel agent techniques and model optimization
File: C:\Users\PGI\Desktop\docs\PARALLEL-AGENTS-ADVANCED.md