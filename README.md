# Reverse Engineering Toolkit

A methodology and tooling guide for systematically reverse engineering software platforms using AI agents.

This toolkit is **plugin-based**: a shared methodology layer applies to any platform, and each platform is a self-contained module under `platforms/`.

## Available platform modules

| Platform | Module | Skill |
|----------|--------|-------|
| ServiceNow | [`platforms/servicenow/`](platforms/servicenow/) | [`skills/servicenow-mcp/SKILL.md`](skills/servicenow-mcp/SKILL.md) |

> Want to add a platform? See [docs/03-contributing.md](docs/03-contributing.md).

## Repository layout

```
docs/                    ← general methodology (platform-agnostic, GitHub-rendered)
  01-methodology.md      ← the 5-step reverse engineering process
  02-mkdocs.md           ← how to build browsable docs from findings (uvx mkdocs build)
  03-contributing.md     ← how to add a new platform module
platforms/               ← platform-specific modules
  servicenow/            ← ServiceNow guide (setup, methodology, prompts, docs)
  _template/             ← skeleton for a new platform — copy this to add yours
skills/                  ← Copilot skill files (load with /skills in Copilot CLI)
  registry.yaml          ← machine-readable skill registry
  servicenow-mcp/        ← ServiceNow skill
template/                ← copy-paste starting point for a new investigation project
AGENTS.md                ← AI agent bootstrap guide (read this if you're an AI agent)
```

## For AI agents

Read [`AGENTS.md`](AGENTS.md) — it lists all available skills and the exact steps to install and configure them (including MCP server setup).

## Quick start

1. Clone this repository and open a Copilot CLI session inside it.
2. Pick a platform from the table above and follow its module's setup guide (`platforms/<platform>/01-setup.md`).
3. Run `/skills` in Copilot CLI to load the platform skill — it teaches Copilot which tools to call and how to run the 5-step workflow.
4. Start investigating.

## How the skills work

Skill files in `skills/` are loaded via `/skills` in Copilot CLI. They teach Copilot:

- Which MCP tools to call for which tasks
- How to run the 5-step reverse engineering workflow
- Which fields to fetch for each artifact type
- How to avoid overloading the context window

The MCP server registration gives Copilot the *capability* to call the tools. The skill file gives it the *knowledge* of how to use them well.

## Contributing

Add a new platform module by following the guide in [docs/03-contributing.md](docs/03-contributing.md) and copying [platforms/_template/](platforms/_template/).
