# Agent Bootstrap Guide

This repository provides a **platform-agnostic reverse engineering toolkit** with platform-specific modules.
Each module includes a skill file that teaches the AI which tools to call and how to run investigations.

## Quick orientation

```
platforms/       ← Platform-specific guides + skill files (SKILL.md)
template/        ← Copy-paste starting point for a new investigation project
README.md        ← General methodology, contributing guide, MkDocs instructions
```

## Available skills

Load skill files via `/skills` in Copilot CLI (GitHub Copilot) or in your AI agent configuration.

| Skill | Location | Platform guide |
|-------|----------|---|
| servicenow-reverse-engineering | [`platforms/servicenow/SKILL.md`](platforms/servicenow/SKILL.md) | [`platforms/servicenow/`](platforms/servicenow/) |

---

## How to set up and use a platform

Each platform module (e.g., `platforms/servicenow/`) contains:

1. **`README.md`** — complete setup and usage guide (start here)
2. **`SKILL.md`** — Copilot skill file (load via `/skills` in Copilot CLI)

### The workflow

1. Read the platform README to understand prerequisites and setup
2. Install the required CLI tools
3. Configure your credentials (instance, API keys, etc.)
4. Register the tool as an MCP server with your AI assistant
5. Load the SKILL.md file (Copilot CLI only)
6. Start investigating with your AI agent

### MCP server vs Skill file

| | What it does | How it's set up |
|---|---|---|
| **MCP server** | Makes tools callable by the AI (e.g., `snow mcp` exposes ServiceNow tools) | One-time config in `~/.copilot/mcp-config.json` or your editor's MCP settings |
| **Skill file** | Teaches the AI *when and how* to use those tools (e.g., the 5-step reverse engineering workflow) | Load via `/skills` in Copilot CLI or equivalent |

You need both. The MCP server provides the capability; the skill teaches the intelligence.

---

## General methodology

All investigations follow the same 5-step process (the skill files automate these):

1. **Size** — count artifacts before downloading anything
2. **Discover** — save an index (IDs + types) to disk
3. **Inspect** — summarise what types are present
4. **Fetch** — read content selectively, one artifact at a time
5. **Document** — write markdown tutorials from your findings

See the [README Methodology section](README.md#methodology) for the full explanation.

---

## Adding a new platform

See [Contributing](README.md#contributing) and copy [`platforms/_template/`](platforms/_template/).
