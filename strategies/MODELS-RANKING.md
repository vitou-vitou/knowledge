# Cursor Models - Full Ranking (Top 1 to Lowest)

> Ranked from strongest / most capable (Top 1) to fastest / lightest. Based on the model list available to this Cursor environment.
> Date: 2026-05-07

---

## Master Ranking Table

| Rank | Model | Tier | Best For | Speed | Reasoning | Cost (rel.) | Notes |
|:---:|---|---|---|:---:|:---:|:---:|---|
| 1 | claude-opus-4-7-thinking-xhigh | Flagship | Hardest debugging, architecture, deep refactors, ambiguous specs | Slow | 10/10 | Highest | Highest reasoning ceiling. Use only when complexity demands it. |
| 2 | claude-4.6-sonnet-medium-thinking | Pro | Daily coding, careful edits, multi-file refactors, planning | Medium | 9/10 | High | Best balance of intelligence and speed. Recommended default. |
| 3 | gpt-5.5-medium | Pro | General reasoning, mixed coding + writing, broad knowledge | Medium | 8.5/10 | High | Strong all-rounder. Good when you want a non-Claude perspective. |
| 4 | gpt-5.3-codex | Specialist | Pure coding, scripting, code completion, repetitive edits | Medium-Fast | 8/10 | Medium | Tuned for code. Slightly older base than 5.5. |
| 5 | composer-2-fast | Fast | Quick edits, simple commands, low-stakes tasks, autocomplete | Very Fast | 6.5/10 | Low | Cheapest. Best when latency matters more than depth. |

---

## Detailed Breakdown (Top 1 -> Lowest)

### 1. claude-opus-4-7-thinking-xhigh - TOP TIER
- Family: Anthropic Claude Opus 4.7
- Mode: Extended thinking, xhigh reasoning budget
- Strength: Deepest chain-of-thought, best at decomposing very complex problems
- Use when:
  - Multi-system refactor across many files
  - Performance/security audits
  - Hard debugging where root cause is non-obvious
  - Designing new architecture from scratch
- Avoid when: Simple edits, fast iteration, cost-sensitive work

### 2. claude-4.6-sonnet-medium-thinking - DAILY DRIVER
- Family: Anthropic Claude Sonnet 4.6
- Mode: Medium thinking budget
- Strength: Excellent code reasoning + faster than Opus
- Use when:
  - Day-to-day feature work
  - Code review and refactoring
  - Planning and design discussions
  - Most agent tasks (this is the recommended general-purpose pick)
- Avoid when: You need maximum reasoning depth (use Opus) OR you only need a one-line fix (use Composer)

### 3. gpt-5.5-medium - STRONG ALTERNATIVE
- Family: OpenAI GPT 5.5
- Mode: Medium reasoning
- Strength: Strong general intelligence, broad world knowledge, good at non-code tasks too
- Use when:
  - You want a second opinion vs. Claude
  - Mixed tasks (code + research + writing)
  - Tasks needing wide knowledge breadth
- Avoid when: Pure deep code refactoring (Sonnet/Opus often edge ahead in code)

### 4. gpt-5.3-codex - CODE SPECIALIST
- Family: OpenAI GPT 5.3 Codex
- Mode: Code-tuned variant
- Strength: Optimized for code generation and completion
- Use when:
  - Boilerplate generation
  - Mechanical code transformations
  - Standard library / framework code
- Avoid when: Tasks requiring deep architectural reasoning or non-code judgment

### 5. composer-2-fast - SPEED TIER
- Family: Cursor Composer 2 (fast variant)
- Mode: Optimized for speed
- Strength: Very low latency, cheap
- Use when:
  - Quick one-off edits
  - Simple commands like "rename this variable"
  - High-volume mechanical tasks
  - Inline autocomplete-style help
- Avoid when: Anything requiring multi-step reasoning, debugging, or architectural decisions

---

## Quick Selection Cheat Sheet

| Your task | Pick |
|---|---|
| "I have a weird bug nobody can find" | 1. claude-opus-4-7-thinking-xhigh |
| "Refactor this module across 10 files" | 1. Opus or 2. Sonnet |
| "Build a new feature end-to-end" | 2. claude-4.6-sonnet-medium-thinking |
| "Plan / discuss / review architecture" | 2. Sonnet (or 1. Opus for huge systems) |
| "Mixed coding + writing + research" | 3. gpt-5.5-medium |
| "Generate a lot of routine code" | 4. gpt-5.3-codex |
| "Rename, format, small tweak" | 5. composer-2-fast |
| "PC cleanup / simple shell tasks" | 5. composer-2-fast or 3. gpt-5.5-medium |
| "Auto mode is on, what do I do?" | Leave Auto on. Switch only when output feels too shallow or too slow. |

---

## Speed vs. Intelligence Map

    Intelligence
    ^
    |  [1] Opus xhigh
    |
    |     [2] Sonnet medium-thinking
    |
    |        [3] GPT 5.5 medium
    |
    |           [4] GPT 5.3 codex
    |
    |                          [5] Composer 2 fast
    +-----------------------------------------> Speed

---

## How to Switch Model in Cursor

1. Open chat panel.
2. Click the model name at the bottom of the input box.
3. Toggle Auto OFF if you want manual control.
4. Pick from the list above.
5. Toggle Auto back ON when finished if you prefer automated routing.

---

## Recommended Defaults

- Daily work: claude-4.6-sonnet-medium-thinking
- Hard problems: claude-opus-4-7-thinking-xhigh
- Speed work: composer-2-fast
- Auto mode: keep ON unless a specific task needs a specific model

---

Prepared: 2026-05-07
File: C:\Users\PGI\Desktop\MODELS-RANKING.md
