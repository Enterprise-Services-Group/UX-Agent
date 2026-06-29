# UX Agent — Orchestra

**Multi-phase UX orchestration system for GitHub Copilot.** 6 specialist agents, a 3-phase pipeline (Strategize → Create → Audit & Ship), and mandatory quality gates. Ships higher-quality design output by running every creative artifact through review before delivery.

## What's New in V2

| V1 (old) | V2 (new) |
|---|---|
| Router — picks ONE agent and stops | Pipeline — runs 3 phases with quality gates |
| 8 bloated agents (150K+ chars) | 6 lean agents (~1,100 lines total) |
| No quality review loop | Mandatory audit + refinement cycle |
| Sequential only | Parallel when independent |
| No handoff protocol | Structured phase transitions |

## Agent Suite

| Agent | Covers |
|---|---|
| **UX Agent** | Orchestrator — plans the pipeline, dispatches specialists, runs quality loops |
| **UX Strategist** | User research, personas, journeys, IA, design thinking, retention strategy |
| **UX Visual** | Visual design, aesthetics, anti-slop, layout, typography, colour, industry styles |
| **UX Interaction** | Motion design, micro-interactions, gestures, animation systems, spring physics |
| **UX Writer** | UX copy, microcopy, content design, voice and tone, error messages |
| **UX Design System** | DTCG tokens, Atomic Design, component quality (8 states), handoff, DESIGN.md |
| **UX Quality** | Usability heuristics, WCAG 2.2 AA accessibility, animation performance, design review |

## Pipeline

```
REQUEST → [Phase 1: STRATEGIZE] → [Phase 2: CREATE] → [Phase 3: AUDIT & SHIP]
              ↓                       ↓                      ↓
         ux-strategist          ux-visual              ux-quality
                                ux-interaction
                                ux-writer
                                ux-design-system
```

- **Phase 1** — Understand the problem. Define users, tone, approach, constraints.
- **Phase 2** — Produce the design. Dispatch 1–4 specialists in parallel where possible.
- **Phase 3** — Audit against usability, accessibility, performance, and anti-slop standards. If Critical issues found, loop back to Phase 2 for refinement (max 2 loops).

## Installation

### Global (available in every workspace)

Copy agent and prompt files into your VS Code user data folder:

**macOS**
```bash
git clone https://github.com/Enterprise-Services-Group/UX-Agent.git
cd UX-Agent
cp .github/agents/ux-*.agent.md ~/Library/Application\ Support/Code/User/agents/
cp .github/prompts/ux.prompt.md ~/Library/Application\ Support/Code/User/prompts/
```

**Windows** (PowerShell)
```powershell
git clone https://github.com/Enterprise-Services-Group/UX-Agent.git
cd UX-Agent
Copy-Item .github\agents\ux-*.agent.md "$env:APPDATA\Code\User\agents\"
Copy-Item .github\prompts\ux.prompt.md "$env:APPDATA\Code\User\prompts\"
```

**Linux**
```bash
git clone https://github.com/Enterprise-Services-Group/UX-Agent.git
cd UX-Agent
cp .github/agents/ux-*.agent.md ~/.config/Code/User/agents/
cp .github/prompts/ux.prompt.md ~/.config/Code/User/prompts/
```

Restart VS Code after copying.

### Workspace-only

To limit the agents to a single project:
```bash
cp -r .github /path/to/your/project/
```

## Usage

### Slash command

In any Copilot Chat panel, type `/ux` followed by your design task:

```
/ux design a landing page for a fintech product
/ux audit this dashboard for usability issues
/ux create a design system with dark mode tokens
/ux write UX copy for our onboarding flow
/ux map the user journey for checkout
/ux add micro-interactions to this component
```

### Agent mode

Switch to **UX Agent** from the Copilot Chat mode dropdown to run a full session
with the orchestrator active throughout.

## Requirements

- VS Code 1.99 or later
- GitHub Copilot extension
- `chat.promptFiles` enabled (on by default in VS Code 1.99+)

If `/ux` doesn't appear, add to `.vscode/settings.json`:
```json
{ "chat.promptFiles": true }
```

## Architecture

See [WORKFLOW.md](WORKFLOW.md) for the full pipeline specification.
See [DESIGN.md](DESIGN.md) for the architectural decisions and tradeoffs.

## Updating

If you installed globally and pull changes:
```bash
cp .github/agents/ux-*.agent.md ~/Library/Application\ Support/Code/User/agents/
cp .github/prompts/ux.prompt.md ~/Library/Application\ Support/Code/User/prompts/
```
