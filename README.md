# ServiceNow Reverse Engineering Toolkit

A methodology guide and project template for systematically reverse engineering ServiceNow
functionality using the `snow` CLI and GitHub Copilot CLI (or Claude Desktop).

Clone this repo, configure the MCP server once, and you can investigate any ServiceNow feature
by asking Copilot to dig through scripts, business rules, and UI artifacts.

## What's in here

```
.github/skills/servicenow-mcp/SKILL.md   ← Copilot CLI skill (auto-loaded)
how-to-reverse-engineer/                  ← Step-by-step methodology guide
template/                                 ← Starter project for a new investigation
```

### The guide

`how-to-reverse-engineer/` walks through the entire process:

| File | Contents |
|------|----------|
| `index.md` | Overview — what this is and what you'll be able to do |
| `01-setup.md` | **Start here** — install snow, configure instance, register MCP |
| `02-methodology.md` | The 5-step reverse engineering process |
| `03-copilot-prompts.md` | Prompt patterns; how the skill works |
| `04-documenting-findings.md` | Structuring findings, MkDocs, publishing |

### The template

`template/` is a copy-paste starting point for a new investigation:

```
template/
├── README.md          ← describe what you're investigating
├── Makefile           ← make build / make start / make clean
├── mkdocs.yml         ← pre-configured, just change site_name
└── markdown/
    └── index.md       ← your findings go here
```

## Quick start

### 1. Prerequisites

- [snow CLI](https://github.com/chovanecm/snow-run-python) — `pipx install "git+https://github.com/chovanecm/snow-run-python@main"`
- [MkDocs](https://www.mkdocs.org/) — `pipx install mkdocs`
- [GitHub Copilot CLI](https://githubnext.com/projects/copilot-cli/) or [Claude Desktop](https://claude.ai/download)

### 2. Add your ServiceNow instance

```bash
snow add --default your-instance.service-now.com
snow login
```

### 3. Register the MCP server (one-time)

Add to `~/.copilot/mcp-config.json`:

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

For Claude Desktop, see [how-to-reverse-engineer/01-setup.md](how-to-reverse-engineer/01-setup.md).

### 4. Clone this repo and start Copilot CLI here

```bash
git clone <this-repo> servicenow-reverse-engineering
cd servicenow-reverse-engineering
gh copilot          # or: copilot
```

The `.github/skills/servicenow-mcp/SKILL.md` file is auto-loaded — Copilot now knows the
full reverse engineering workflow.

### 5. Start investigating

```
Reverse engineer the 'approval' functionality.
Study sys_metadata, download the relevant scripts, and write me a tutorial.
```

Read the full guide starting at [how-to-reverse-engineer/index.md](how-to-reverse-engineer/index.md).

## How the skill works

The skill file (`.github/skills/servicenow-mcp/SKILL.md`) is automatically loaded by
GitHub Copilot CLI when you work in this directory. It teaches Copilot:

- Which `snow` tools to call for discovery vs inspection vs script fetching
- How to avoid overloading the AI context window (output files, field projection)
- The 5-step workflow: size → discover → inspect → fetch → document
- Which fields are most useful for each artifact type (business rules, script includes, etc.)

The MCP server registration (step 3 above) gives Copilot the *capability* to call `snow`.
The skill file gives it the *knowledge* of how to use it well.
