# <PLATFORM> — Reverse Engineering Module

<!-- TODO: Replace <PLATFORM> with your platform name throughout this file -->

A guide for systematically reverse engineering `<PLATFORM>` features using AI assistance.

This module implements the [general 5-step methodology](../../README.md#methodology) with `<PLATFORM>`-specific tooling.

## Prerequisites

<!-- TODO: List what someone needs before they can use this module -->

- Access to a `<PLATFORM>` instance
- `<CLI tool>` installed — see [01-setup.md](01-setup.md)
- GitHub Copilot CLI or another MCP-compatible AI agent
- *(optional)* `uv` for building documentation — see [Building docs](../../README.md#building-docs-from-findings)

## Quick start

```bash
# 1. Install the platform CLI
<install command>

# 2. Configure credentials
<config command>

# 3. Register MCP (GitHub Copilot CLI)
# See 01-setup.md for full instructions
```

Then start an investigation:

```
Reverse engineer the '<feature>' functionality.
```

## Contents

| File | What it covers |
|------|---------------|
| [01-setup.md](01-setup.md) | Install tools, configure instance, wire up AI |
| [02-methodology.md](02-methodology.md) | The 5-step process applied to `<PLATFORM>` |

## Skill

<!-- TODO: If you created a Copilot skill, link it here -->

The skill in [`SKILL.md`](SKILL.md) can be loaded via `/skills` in Copilot CLI.
