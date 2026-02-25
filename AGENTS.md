# Agent Bootstrap Guide

This repository provides a **platform-agnostic reverse engineering toolkit** with platform-specific modules.
Each module comes with a Copilot skill that teaches the AI which tools to call and how to run investigations.

## Quick orientation

```
platforms/       ← Platform-specific guides + skill files (SKILL.md per platform)
template/        ← Copy-paste starting point for a new investigation project
README.md        ← General methodology, contributing guide, MkDocs instructions
```

## Available skills

Skill files are loaded via `/skills` in Copilot CLI.

| Skill | Skill file | Platform guide |
|-------|-----------|----------------|
| `servicenow-mcp` | [`platforms/servicenow/SKILL.md`](platforms/servicenow/SKILL.md) | [`platforms/servicenow/`](platforms/servicenow/) |

---

## Platform setup

Each platform module provides its own skill and setup instructions:

1. **Install the platform-specific CLI/tool** (see the platform's `README.md`)
2. **Configure your instance or environment** (see the platform's `01-setup.md`)
3. **Register the CLI/tool as an MCP server** (see `01-setup.md`)
4. **Load the skill** via `/skills` and start your investigation

#### Example: ServiceNow

See [`platforms/servicenow/`](platforms/servicenow/) for full setup and investigation steps.

---

## General methodology

Regardless of platform, all investigations follow the same 5-step process:

1. **Size** — count artifacts before downloading anything
2. **Discover** — save an index (IDs + types) to disk
3. **Inspect** — summarise what types are present
4. **Fetch** — read content selectively, one artifact at a time
5. **Document** — write markdown tutorials from your findings

See the [README Methodology section](README.md#methodology) for the full explanation.

---

## Adding a new platform

See [Contributing](README.md#contributing) and copy [`platforms/_template/`](platforms/_template/).
