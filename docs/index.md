# Reverse Engineering Toolkit

A methodology and tooling guide for systematically reverse engineering software platforms using AI agents.

This toolkit is **plugin-based**: a shared methodology layer applies to any platform, and each platform is a self-contained module under [`platforms/`](../platforms/).

## Available platform modules

| Platform | Module | Skill |
|----------|--------|-------|
| ServiceNow | [`platforms/servicenow/`](../platforms/servicenow/) | [`skills/servicenow-mcp/`](../skills/servicenow-mcp/SKILL.md) |

## How it works

```
┌─────────────────────────────────────┐
│  General methodology (this layer)   │  ← platform-agnostic principles
│  docs/01-methodology.md             │
└─────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│  Platform module                    │  ← platform-specific setup + steps
│  platforms/<platform>/              │
└─────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│  Copilot skill                      │  ← teaches the AI agent what tools to call
│  skills/<platform>-*/SKILL.md       │
└─────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│  Investigation project              │  ← your reverse-engineered findings
│  (copy from template/)              │
└─────────────────────────────────────┘
```

## Contents of this guide

| File | What it covers |
|------|---------------|
| [01-methodology.md](01-methodology.md) | The general 5-step reverse engineering process |
| [02-mkdocs.md](02-mkdocs.md) | Building browsable docs from your findings |
| [03-contributing.md](03-contributing.md) | Adding a new platform module |

## For AI agents

See [`AGENTS.md`](../AGENTS.md) at the repo root — it lists all available skills and the exact setup steps required for each platform.
