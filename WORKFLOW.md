# WORKFLOW.md — UX Agent V2 Pipeline

## Architecture

The UX Agent is a **multi-phase orchestration system** that runs design tasks through
a pipeline of 6 specialist agents with mandatory quality gates.

```
REQUEST → [Phase 1: STRATEGIZE] → [Phase 2: CREATE] → [Phase 3: AUDIT & SHIP]
              ↓                       ↓                      ↓
         ux-strategist          ux-visual              ux-quality
         (Design Thinking,      ux-interaction         (Nielsen 10 + Norman 7
          UX research methods,   ux-writer              + Shneiderman 8,
          personas, journeys,    ux-design-system       WCAG 2.2 AA, Gestalt,
          service blueprints,                           animation perf,
          IA, metrics,                                  content quality,
          content quality)                              anti-slop, service audit)
```

## Agent Inventory V2

| Agent | Domain | Best Practices Incorporated | Lines |
|---|---|---|---|
| **ux-agent** | Orchestrator — phase manager, pipeline controller | 3-phase pipeline, parallel dispatch, refinement loops | ~220 |
| **ux-strategist** | Strategy, research, IA, service design, content | Design Thinking, UX research phases, Service Design (People/Props/Processes), UX metrics, Flesch readability, find→act gap analysis | ~260 |
| **ux-visual** | Visual design, aesthetics, anti-slop | Hallmark 6 disciplines, 20 named themes, 54-system reference library, DESIGN.md integration, Claude Design anti-slop, popular-web-designs industry aesthetics | ~380 |
| **ux-interaction** | Motion, gestures, animation | Gesture library (mobile/desktop/keyboard), spring physics, loading patterns by duration, feedback systems, FLIP technique, reduced-motion strategy, Shneiderman progressive disclosure | ~290 |
| **ux-writer** | UX writing, content design, microcopy | Flesch-Kincaid readability, find→act gap detection, content freshness, voice/tone spectrum, error message formula, banned AI copy list, WCAG accessibility in writing | ~300 |
| **ux-design-system** | Tokens, components, handoff, DESIGN.md | DTCG 3-tier token architecture, Google DESIGN.md spec format (9 sections), 6-gate component quality, 8 states, fidelity ladder L1–L5, design review rubric, Tailwind/DTCG export, multi-framework output (React/Next.js/SwiftUI) | ~400 |
| **ux-quality** | Usability, a11y, performance, content audit | Nielsen 10 + Norman 7 + Shneiderman 8, Gestalt 5 laws, WCAG 2.2 AA full audit, animation performance checklist, content quality audit, anti-slop checklist, service design audit, weighted scoring | ~390 |

**Total system size:** ~2,240 lines (up from 1,121 in V2-initial, now significantly richer).

## What Changed in V2.1

| Agent | Key Additions |
|---|---|
| ux-strategist | Design Thinking phases, UX research methods (Discover→Explore→Test→Listen), Service Design framework (People/Props/Processes), UX metrics, content quality dimensions |
| ux-visual | Hallmark 6 disciplines, 20 named themes (from Hallmark + popular-web-designs), 54-system reference library, DESIGN.md integration, expanded anti-slop rules |
| ux-interaction | Gesture library (mobile/desktop/keyboard), loading patterns by duration, FLIP technique, expanded feedback systems, View Timeline API, keyboard gesture map |
| ux-writer | Flesch-Kincaid readability metrics, find→act gap analysis, content freshness, banned words expanded (11 items with replacements), accessibility in writing (WCAG), error message formula |
| ux-design-system | Google DESIGN.md spec (9-section format with YAML), DTCG export, multi-framework output (React+Tailwind v4, Next.js 15, SwiftUI 6), design review rubric, spec QA gate |
| ux-quality | Norman's 7 principles, Shneiderman's 8 golden rules, Gestalt 5 laws, WCAG 2.2 AA PRCOA framework, content quality audit (readability/gap/freshness/duplication), service design audit, anti-slop checklist (12 items) |
| ux-agent | Specialist capability summary, edge case routing, expanded phase selection, service design pipeline |

## Design Principles

1. **Routing, not doing.** The orchestrator never produces design output.
2. **Lean but rich.** Each specialist carries domain knowledge but stays focused.
3. **Structured handoffs.** Phase boundaries are explicit with clear I/O contracts.
4. **Quality as a phase, not an afterthought.** Audit is mandatory for all creative output.
5. **Limited loops.** Max 2 refinement cycles to prevent infinite iteration.
6. **Parallel where possible.** Independent work runs concurrently.
7. **Evidence-first.** Strategy is grounded in research methods, not assumptions.
8. **Standards-based.** All quality checks reference established standards (Nielsen, Norman, WCAG, Gestalt).

## Extension Guide

### Adding a New Specialist

1. Create `.github/agents/ux-{domain}.agent.md` with frontmatter:
   ```yaml
   ---
   name: UX {Domain}
   description: "..."  # Keep description under 500 chars, list trigger words
   tools: [read, edit] # Minimal tool set
   user-invocable: false
   ---
   ```
2. Keep the agent under 500 lines. Reference external knowledge rather than embedding it.
3. Add routing rules to ux-agent and ux.prompt.md.
4. Document the new specialist in this file.

### Modifying Pipeline Phases

The pipeline is defined in three places (keep them in sync):
1. `ux-agent.agent.md` — full orchestrator logic
2. `ux.prompt.md` — slash command version (compact)
3. This file — documentation
