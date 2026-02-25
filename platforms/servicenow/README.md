# ServiceNow — Reverse Engineering Module

A guide for systematically reverse engineering ServiceNow functionality using the `snow` CLI and GitHub Copilot (or Claude Desktop).

This module implements the [general 5-step methodology](../../docs/01-methodology.md) with ServiceNow-specific tooling.

## What you'll be able to do

- Explore any ServiceNow instance's custom functionality without needing source access or vendor docs
- Identify all artifacts (tables, script includes, business rules, UI actions, transform maps) that make a feature work
- Read and understand the actual JavaScript business logic
- Produce browsable documentation your team can reference

## Prerequisites

- Access to a ServiceNow instance (any role that can read metadata)
- Python 3.8+ and `pipx` installed
- GitHub Copilot CLI — or Claude Desktop
- 30 minutes for setup, then as long as you want to investigate

## Quick start

```bash
# 1. Install the snow CLI
pipx install "git+https://github.com/chovanecm/snow-run-python@main"

# 2. Configure your instance
snow add --default your-instance.service-now.com
snow login

# 3. Register the MCP server (GitHub Copilot CLI)
# Edit ~/.copilot/mcp-config.json — see 01-setup.md for full instructions
```

Then start an investigation:

```
Reverse engineer the 'approval' functionality.
Study sys_metadata, download the relevant scripts, and write me a tutorial.
```

## Contents

| File | What it covers |
|------|---------------|
| [01-setup.md](01-setup.md) | Install snow, configure instance, wire up AI |
| [02-methodology.md](02-methodology.md) | The 5-step process applied to ServiceNow artifacts |
| [03-copilot-prompts.md](03-copilot-prompts.md) | ServiceNow-specific prompt examples |

For documentation structure and MkDocs, see [`docs/02-mkdocs.md`](../../docs/02-mkdocs.md).
For general prompting tips, see [`docs/04-prompting.md`](../../docs/04-prompting.md).

## Skill

The `servicenow-mcp` skill in [`SKILL.md`](SKILL.md)
can be loaded via `/skills` in Copilot CLI.
It teaches Copilot which `snow` tools to call, the 5-step workflow, and how to avoid overloading the context window.
