# ServiceNow — Reverse Engineering Module

Use this as the **single flow** to reverse engineer a ServiceNow feature and produce structured documentation.

## What this module includes

- This file: end-to-end action flow (install → connect → investigate → document)
- [`SKILL.md`](SKILL.md): Copilot skill that teaches tool selection and workflow

## Action flow

### 1) Install the CLI

```bash
pipx install "git+https://github.com/chovanecm/snow-run-python@main"
```

Verify:

```bash
snow --help
```

### 2) Configure your instance

```bash
snow add --default your-instance.service-now.com
snow login
snow record count incident
```

If the final command returns a count, your connection works.

### 3) Register MCP server (one-time per machine)

Configure your AI client to run:

- command: `snow`
- args: `mcp`

For Copilot CLI, add an MCP server named `servicenow` and then load skills via `/skills`.

### 4) Load the skill

Load [`SKILL.md`](SKILL.md) in Copilot CLI.  
You need both MCP + skill:

- MCP: exposes `snow` tools
- Skill: instructs the AI when and how to use them for reverse engineering

### 5) Run the investigation workflow

Use this order for any feature keyword:

1. **Size** — count matching artifacts (`sys_metadata`)
2. **Discover** — save IDs + types to disk
3. **Inspect** — summarize artifact types from the saved index
4. **Fetch** — read relevant artifacts selectively, one at a time
5. **Document** — write markdown tutorial files from findings

Starter prompt:

> Reverse engineer the `<feature-name>` functionality in ServiceNow. Size first, then save an index of artifact IDs/types to disk, inspect types, fetch relevant scripts selectively, and write tutorial docs.

### 6) Publish structured docs

Use root template:

```bash
cd /path/to/ai-reverse-engineer
cp -r template tutorials/<feature-name>
cd tutorials/<feature-name>
make build
```

Open `docs-html/index.html`.

## Notes

- Keep large responses out of context; save large searches to files and read selectively.
- Focus on active artifacts first (`active=true`).
- Cross-reference script include names across business rules and UI actions.
