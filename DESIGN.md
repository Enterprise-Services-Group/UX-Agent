# DESIGN.md — UX Agent System Design V2.1

## Philosophy

The UX Agent is built on the principle that **AI-assisted design needs orchestration,
not just generation**. A single AI producing a one-shot design is fast but mediocre.
A pipeline of specialists with mandatory review produces higher-quality output with
only marginally more time.

V2.1 adds depth: every agent now carries best-practice knowledge from established
design disciplines — Nielsen, Norman, Shneiderman, Gestalt, DTCG, WCAG, Hallmark,
and Google's DESIGN.md spec.

## Design Decisions

### 1. Pipeline, not router
V1 routed to ONE agent and stopped. V2 runs a multi-phase pipeline. Quality cannot
be achieved in one pass. V2.1 adds service design and content quality to the pipeline.

### 2. Six specialists, deeply informed
V2 had 6 lean agents. V2.1 enriches each with best-practice knowledge:

| Agent | Key References |
|---|---|
| ux-strategist | Design Thinking, Service Design (People/Props/Processes), UX Research phases, UX metrics, content quality |
| ux-visual | Hallmark 6 disciplines, 20 named themes, 54-system library, Claude Design, popular-web-designs |
| ux-interaction | Gesture library, spring physics, loading patterns, FLIP, reduced-motion |
| ux-writer | Flesch-Kincaid, find→act gap, content freshness, error formula, banned copy |
| ux-design-system | DTCG, Google DESIGN.md, 6-gate quality, 8 states, multi-framework |
| ux-quality | Nielsen 10, Norman 7, Shneiderman 8, Gestalt 5, WCAG 2.2 AA, content audit |

### 3. Standards-based, not opinion-based
Every quality check references an established standard. Every heuristic has a name and
number. Every accessibility check maps to a WCAG criterion. This makes findings
actionable and defensible.

### 4. Mandatory audit
V1 had quality as a specialist you could route to. V2/V2.1 has quality as a phase
every creative output must pass through.

### 5. Structured handoffs
Explicit handoff markers (`[STRATEGY BRIEF READY]`, `[AUDIT COMPLETE — X/100]`)
let the orchestrator track pipeline state.

### 6. Content quality as a first-class concern
V2.1 adds readability (Flesch-Kincaid), actionability (find→act gap), freshness
(staleness detection), and duplication as standard audit dimensions.

### 7. Service design, not just screen design
V2.1 adds People/Props/Processes framework for multi-touchpoint service analysis.

## Tradeoffs

| Choice | Benefit | Cost |
|---|---|---|
| Rich agents (~300-400 lines each) | Deep domain knowledge, standards-based output | Larger context windows per agent |
| 6 agents vs more/fewer | Balanced coverage without excessive overhead | Some domain blending |
| Mandatory audit on all creative output | Catches slop, a11y, perf issues consistently | Extra pass on every creative task |
| GitHub Copilot format | Works in Copilot Chat and agent mode | Can't implement true parallel execution |
| Standards-based (vs generative) | Predictable, defensible quality | Can't adapt to novel patterns |

## Future

- **Claude Code integration:** The pipeline maps naturally to Workflow scripts.
- **Hermes integration:** Each specialist could be a Hermes skill with MCP tooling.
- **Figma integration:** Phase 2 could pull from and push to Figma files.
- **Real parallelism:** With Claude Code or Hermes subagents, Phase 2 can truly
  run in parallel rather than sequential dispatch.
- **Automated DESIGN.md validation:** Integrate `npx @google/design.md lint` into
  Phase 3 for automated token spec verification.
