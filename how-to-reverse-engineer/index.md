# Reverse Engineering ServiceNow Functionality

This guide teaches you how to systematically investigate, understand, and document
any ServiceNow application or feature — using AI assistance and the `snow` CLI tool.

## What you'll be able to do

After following this guide you can:

- Explore any ServiceNow instance's custom functionality without needing source access or vendor docs
- Identify all artifacts (tables, script includes, business rules, UI actions, transform maps) that make a feature work
- Read and understand the actual JavaScript business logic
- Produce browsable documentation your team can reference

## What this workflow looks like

```
ServiceNow instance
        │
        │  snow CLI (MCP mode)
        ▼
GitHub Copilot CLI  ──────────────────────────────────────────────────────────────────────
        │                                                                                  │
  asks questions:                                                               you direct it:
  "how many <feature> artifacts?"                                      "reverse engineer <feature>"
  "fetch script include <ScriptIncludeName>"                           "document the update rules"
  "what tables does this feature use?"                                   "write a tutorial file"
        │
        ▼
  markdown/          ←  source documentation you edit
  mkdocs.yml         ←  build config
  html/              ←  open index.html in any browser
```

## Who this is for

| Role | What you get |
|------|-------------|
| **Developer** | Understand business logic, find integration points, reproduce bugs |
| **Admin** | Audit what a feature does before changes, document for compliance |
| **New team member** | Get up to speed on a feature that has no documentation |
| **Architect** | Evaluate whether to extend vs recreate on another instance |

## Contents of this guide

| File | What it covers |
|------|---------------|
| [01-setup.md](01-setup.md) | Install tools, configure your instance, wire up AI |
| [02-methodology.md](02-methodology.md) | The 5-step reverse engineering process |
| [03-copilot-prompts.md](03-copilot-prompts.md) | Prompt patterns and the servicenow-mcp skill |
| [04-documenting-findings.md](04-documenting-findings.md) | Document structure, MkDocs, building the site |

## Worked example

The companion example repository contains a fully reverse-engineered ServiceNow application
documented using this exact workflow — covering tables, script includes, business rules, UI actions,
and transform maps.

## Prerequisites

- Access to a ServiceNow instance (any role that can read metadata)
- Python 3.8+ and `pipx` installed
- GitHub Copilot CLI (the tool you're reading this in) — or Claude Desktop
- 30 minutes for setup, then as long as you want to investigate
