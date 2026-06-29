# WORKFLOW.md — UX Agent Pipeline

## Architecture

The UX Agent is a **multi-phase orchestration system**, not a simple router. It runs
design tasks through a pipeline of specialist agents with mandatory quality gates.

```
REQUEST → [Phase 1: STRATEGIZE] → [Phase 2: CREATE] → [Phase 3: AUDIT & SHIP]
              ↓                       ↓                      ↓
         ux-strategist          ux-visual              ux-quality
         (research, IA,         ux-interaction         (heuristics, a11y,
          personas, tone)       ux-writer               perf, review)
                                ux-design-system
```

## Agent Inventory

| Agent | Domain | Size | Tools |
|---|---|---|---|
| **ux-agent** | Orchestrator — phase manager, pipeline controller | ~150 lines | agent, read, search, web, todo |
| **ux-strategist** | Strategy, research, IA, journeys | ~130 lines | read, search, web |
| **ux-visual** | Visual design, aesthetics, anti-slop | ~160 lines | read, edit, web |
| **ux-interaction** | Motion, gestures, animation | ~150 lines | read, edit |
| **ux-writer** | UX writing, content design, microcopy | ~160 lines | read, edit |
| **ux-design-system** | Tokens, components, handoff, specs | ~200 lines | read, edit, web |
| **ux-quality** | Usability, a11y, performance, audit | ~170 lines | read, edit, web |

**Total system size:** ~1,120 lines (down from ~750+ lines of bloated agents in V1).

## Phase Definitions

### Phase 1 — STRATEGIZE
**When:** Any non-trivial design task. Skip for purely cosmetic requests.
**Agent:** ux-strategist
**Input:** Original user request + any project docs
**Output:** Strategy brief (users, tone, approach, constraints, risks)
**Gate:** If critical unknowns exist, ask 1–2 clarifying questions before Phase 2.

### Phase 2 — CREATE
**When:** Always (this is where the design artifact is produced).
**Agents:** 1–4 specialists, dispatched based on intent.
**Input:** Original request + strategy brief + DESIGN.md (if exists)
**Parallelism:** Independent specialists run in parallel. Dependent ones run sequentially.
**Output:** Design artifact (code, spec, copy, animation system, or component library).

### Phase 3 — AUDIT & SHIP
**When:** Always for creative output. Skip for research/strategy-only tasks.
**Agent:** ux-quality
**Input:** Original request + strategy brief + Phase 2 output
**Output:** Audit report with score, findings (Critical/Major/Minor), and ship verdict.
**Refinement loop:** If Critical findings exist, return to Phase 2 with audit context
(max 2 loops). Re-run only the specialists whose output had Critical issues.

## Handoff Protocol

Between phases, context is passed as structured metadata:

```
From Phase 1 → Phase 2:
  [STRATEGY BRIEF READY]
  {full strategy brief}

From Phase 2 → Phase 3:
  [DESIGN READY — handoff to Phase 3 audit]
  {complete output + declared design parameters}

From Phase 3 → Ship (or back to Phase 2):
  [AUDIT COMPLETE — X/100 — Critical: N]
  {full audit report}
```

## Quality Standards

All creative output must meet these minimums:
- No BANNED fonts (Inter, Roboto, Arial, system-ui, Space Grotesk)
- No AI slop tells (teal accent, container soup, 3-column features, Lucide defaults)
- WCAG AA contrast (≥ 4.5:1 text, ≥ 3:1 non-text)
- Responsive at 3+ breakpoints
- All interactive elements have focus states
- Empty, loading, and error states addressed

## Extension Guide

### Adding a New Specialist

1. Create `.github/agents/ux-{domain}.agent.md` with frontmatter:
   ```yaml
   ---
   name: UX {Domain}
   description: "..."  # Keep under 500 chars
   tools: [read, edit] # Minimal tool set
   user-invocable: false
   ---
   ```
2. Keep the agent under 200 lines. Reference external knowledge rather than embedding it.
3. Add routing rules to ux-agent and ux.prompt.md.
4. Document the new specialist in this file.

### Modifying Pipeline Phases

The pipeline is defined in three places (keep them in sync):
1. `ux-agent.agent.md` — full orchestrator logic
2. `ux.prompt.md` — slash command version (compact)
3. This file — documentation

## Design Principles

1. **Routing, not doing.** The orchestrator never produces design output.
2. **Lean agents.** Each specialist carries only its own domain knowledge.
3. **Structured handoffs.** Phase boundaries are explicit with clear I/O contracts.
4. **Quality as a phase, not an afterthought.** Audit is mandatory, not optional.
5. **Limited loops.** Max 2 refinement cycles to prevent infinite iteration.
6. **Parallel where possible.** Independent work runs concurrently.
