# UX Agent for GitHub Copilot

An integrated UX design system for GitHub Copilot — 8 specialist sub-agents and a `/ux` slash command that routes automatically to the right expertise.

## What's included

| Agent | Covers |
|---|---|
| **UX Agent** | Master orchestrator — routes all requests |
| **UX Visual** | Aesthetics, anti-slop, 67 style registry, spec-first, DESIGN.md, Impeccable commands |
| **UX Process** | Design sprint, journey architect, AIDA, learning design, information architecture |
| **UX Quality** | Usability audits, Nielsen/Krug, Gestalt, Shneiderman's mantra, interaction cost, axe-core |
| **UX Retention** | Hook Model, Peak-End Rule, engagement loops, emotional feedback |
| **UX Mobile** | iOS/HIG, SwiftUI, App Store, thumb zone, gesture patterns |
| **UX Brand** | Brand identity, Figma-to-code, motion design, theme systems |
| **UX Design System** | DTCG tokens, Atomic Design, component quality, fidelity ladder, handoff checklist |

---

## Installation

### Global (available in every workspace)

Copy the agent and prompt files into your VS Code user data folder so the agents and `/ux` slash command work in any project.

**macOS**
```bash
# Clone the repo
git clone https://github.com/Enterprise-Services-Group/UX-Agent.git
cd UX-Agent

# Copy agents and prompt
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

> The `agents/` and `prompts/` folders will be created automatically if they don't exist. Restart VS Code after copying.

---

### Workspace-only

To limit the agents to a single project, copy the `.github/` folder into your workspace root:

```bash
cp -r .github /path/to/your/project/
```

---

## Usage

### Slash command

In any Copilot Chat panel, type `/ux` followed by your design task:

```
/ux audit this dashboard for usability issues
/ux design a landing page for a fintech product
/ux create a dark mode token system using OKLCH
/ux build a user journey for the checkout flow
/ux what style should I use for a healthcare app
/ux my users aren't coming back after day 3
/ux set up Atomic Design component architecture
```

The slash command routes to the correct specialist sub-agent automatically.

### Agent mode

Switch to **UX Agent** from the Copilot Chat mode dropdown (the pencil/mode selector) to run a full session with the orchestrator active throughout.

---

## Updating

If you installed globally and later pull changes from this repo, re-run the copy commands to sync:

```bash
# macOS
cp .github/agents/ux-*.agent.md ~/Library/Application\ Support/Code/User/agents/
cp .github/prompts/ux.prompt.md ~/Library/Application\ Support/Code/User/prompts/
```

---

## Requirements

- VS Code 1.99 or later
- GitHub Copilot extension
- `chat.promptFiles` enabled (on by default in VS Code 1.99+)

If `/ux` doesn't appear in the slash command autocomplete, add this to `.vscode/settings.json`:
```json
{
  "chat.promptFiles": true
}
```
