# Agent Bootstrap Guide

This repository provides a **platform-agnostic reverse engineering toolkit** with platform-specific modules.
Each module comes with a Copilot skill that teaches the AI which tools to call and how to run investigations.

## Quick orientation

```
skills/          ← Copilot skill files (auto-loaded by GitHub Copilot CLI)
platforms/       ← Platform-specific setup and methodology guides
docs/            ← General methodology (applies to all platforms)
template/        ← Copy-paste starting point for a new investigation project
```

## Available skills

Skill files in `skills/` are automatically loaded by GitHub Copilot CLI when you work in this directory.

| Skill | Skill file | Platform guide |
|-------|-----------|----------------|
| `servicenow-mcp` | [`skills/servicenow-mcp/SKILL.md`](skills/servicenow-mcp/SKILL.md) | [`platforms/servicenow/`](platforms/servicenow/) |

See [`skills/registry.yaml`](skills/registry.yaml) for a machine-readable version of this table.

---

## Platform setup instructions

### ServiceNow

**Skill:** `servicenow-mcp` — reverse engineer ServiceNow features, query tables, run scripts.

#### Step 1 — Install the `snow` CLI

```bash
pipx install "git+https://github.com/chovanecm/snow-run-python@main"
```

Verify:
```bash
snow --help
```

#### Step 2 — Configure your ServiceNow instance

```bash
snow add --default your-instance.service-now.com
snow login
```

#### Step 3 — Register `snow` as an MCP server

**GitHub Copilot CLI** — run inside a Copilot CLI session:

```
/mcp add servicenow
```

When prompted:
- **Type:** `local`
- **Command:** `snow`
- **Args:** `mcp`

Or edit `~/.copilot/mcp-config.json` directly:

```json
{
  "mcpServers": {
    "servicenow": {
      "type": "local",
      "command": "snow",
      "args": ["mcp"],
      "tools": ["*"]
    }
  }
}
```

**Claude Desktop** — add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)
or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "servicenow": {
      "command": "snow",
      "args": ["mcp"]
    }
  }
}
```

**Other MCP-compatible editors** — use `snow mcp` as the server command. Consult your editor's MCP docs.

#### Step 4 — Start an investigation

With the skill loaded and MCP configured:

```
Reverse engineer the 'approval' functionality.
Study sys_metadata, download relevant scripts, and write a tutorial.
```

Full guide: [`platforms/servicenow/`](platforms/servicenow/)

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
