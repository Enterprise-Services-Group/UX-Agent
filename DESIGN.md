# DESIGN.md — UX Agent System Design

## Philosophy

The UX Agent is built on the principle that **AI-assisted design needs orchestration,
not just generation**. A single AI producing a one-shot design is fast but mediocre.
A pipeline of specialists with mandatory review produces higher-quality output with
only marginally more time.

## Design Decisions

### 1. Pipeline, not router
V1 routed to ONE agent and stopped. V2 runs a multi-phase pipeline. This is the
single most impactful architectural change. Quality cannot be achieved in one pass.

### 2. Six specialists, not eight
V1 had 8 agents: visual, process, quality, retention, mobile, brand, design-system,
implementation. V2 consolidates:

| V1 Agent | V2 Mapping | Rationale |
|---|---|---|
| ux-process | → ux-strategist | Process IS strategy — merged |
| ux-retention | → ux-strategist | Retention strategy, not a separate domain |
| ux-mobile | → ux-visual | Mobile is visual design with constraints |
| ux-brand | → ux-visual + ux-design-system | Brand = visual identity + tokens |
| ux-implementation | → ux-quality | Implementation checks = quality audit |
| ux-visual | → ux-visual | Unchanged |
| ux-quality | → ux-quality | Expanded to include a11y, perf, slop |
| ux-design-system | → ux-design-system | Simplified |

**New:** ux-interaction (motion/animation/gestures) and ux-writer (copy/content).

### 3. Lean agents
V1's ux-visual.agent.md was ~150K characters — impossibly bloated for context windows.
V2 agents target 3–7K characters each. Reference knowledge is cited, not embedded.

### 4. Mandatory audit
V1 had quality as a specialist you could route to. V2 has quality as a phase every
creative output must pass through. This is the quality gate.

### 5. Structured handoffs
V1 had no handoff protocol — agents were siloed. V2 uses explicit handoff markers
(`[STRATEGY BRIEF READY]`, `[AUDIT COMPLETE]`) so the orchestrator can track state.

### 6. Parallel execution
V1 ran multi-intent requests sequentially. V2 dispatches independent specialists in
parallel (ux-visual + ux-writer can run together).

### 7. Refinement loop
V1 delivered first-pass output as final. V2 loops back to Phase 2 when the audit
finds Critical issues (max 2 loops). This mirrors real design review cycles.

## Tradeoffs

| Choice | Benefit | Cost |
|---|---|---|
| Pipeline vs. direct routing | Higher quality, consistent output | Slightly slower for simple requests |
| 6 agents vs. 8 | Less context bloat, clearer domains | Some domain blending (mobile in visual) |
| Mandatory audit | Catches slop, accessibility, perf issues | Extra pass on every creative task |
| GitHub Copilot format | Works in Copilot Chat and agent mode | Can't implement true parallel execution |
| No generative AI in agents | Predictable, rule-based quality | Can't adapt to novel design patterns |

## Future

- **Claude Code integration:** The pipeline maps naturally to Workflow scripts.
- **Hermes integration:** Each specialist could be a Hermes skill.
- **Figma integration:** Phase 2 could pull from and push to Figma files.
- **Real parallelism:** With Claude Code or Hermes subagents, Phase 2 can truly
  run in parallel rather than sequential dispatch.
