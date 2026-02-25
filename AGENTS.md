# Agent Bootstrap Guide

This repository provides a **platform-agnostic reverse engineering toolkit** with platform-specific modules.
Each module comes with a Copilot skill that teaches the AI which tools to call and how to run investigations.

## Quick orientation

```
platforms/       ← Platform-specific guides + skill files (SKILL.md per platform)
skills/          ← Machine-readable skill registry (registry.yaml)
docs/            ← General methodology (applies to all platforms)
template/        ← Copy-paste starting point for a new investigation project
```

## Available skills

Skill files in `skills/` are loaded via `/skills` in Copilot CLI.

| Skill | Skill file | Platform guide |
|-------|-----------|----------------|
| `servicenow-mcp` | [`platforms/servicenow/SKILL.md`](platforms/servicenow/SKILL.md) | [`platforms/servicenow/`](platforms/servicenow/) |

See [`skills/registry.yaml`](skills/registry.yaml) for a machine-readable version of this table.

---

## Platform setup instructions

### How to add/setup a platform skill

Each platform module provides its own skill and setup instructions. Follow the steps below for any platform:

1. **Install the platform-specific CLI/tool** (see the platform's README).
2. **Configure your instance or environment** (see platform docs).
3. **Register the CLI/tool as an MCP server** (see platform skill docs).
4. **Start your investigation** (see platform methodology).

#### Example: ServiceNow

**Skill:** `servicenow-mcp` — reverse engineer ServiceNow features, query tables, run scripts.

See [`platforms/servicenow/`](platforms/servicenow/) for detailed ServiceNow setup and investigation steps.

---

## General methodology

Regardless of platform, all investigations follow the same 5-step process:

1. **Size** — count artifacts before downloading anything
2. **Discover** — save an index (IDs + types) to disk
3. **Inspect** — summarise what types are present
4. **Fetch** — read content selectively, one artifact at a time
5. **Document** — write markdown tutorials from your findings

See [`docs/01-methodology.md`](docs/01-methodology.md) for the full explanation.

---

## Adding a new platform

See [`docs/03-contributing.md`](docs/03-contributing.md) and copy [`platforms/_template/`](platforms/_template/).
