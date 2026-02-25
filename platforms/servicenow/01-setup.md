# Setup — ServiceNow

Get the `snow` CLI installed, your instance configured, and the AI assistant connected.

> **General methodology:** This guide is ServiceNow-specific. For the platform-agnostic principles, see [`docs/01-methodology.md`](../../docs/01-methodology.md).

## 1. Install the snow CLI

```bash
pipx install "git+https://github.com/chovanecm/snow-run-python@main"
```

Verify:
```bash
snow --help
```

> **What is `snow`?** It's a CLI tool for interacting with ServiceNow — running background scripts,
> querying tables, inspecting schemas. It also runs as an MCP server so AI assistants can call it
> as tools on your behalf.

## 2. Add your ServiceNow instance

```bash
snow add --default your-instance.service-now.com
```

This prompts for your username and password, stores them securely in the OS keyring, and sets
the instance as default. You can add multiple instances and switch between them.

```bash
# List configured instances
snow list

# Switch default
snow use other-instance.service-now.com

# One-off override
snow --instance other-instance.service-now.com login
```

## 3. Test the connection

```bash
# Login and persist session cookies
snow login

# Quick sanity check — count records in a well-known table
snow record count incident
```

If you get a number back, you're connected.

## 4. Register the MCP server (one-time, per machine)

`snow` exposes its ServiceNow tools as an MCP server. You need to register it once with your
AI assistant so it can call `snow` on your behalf.

### GitHub Copilot CLI

Inside a Copilot CLI session, run:

```
/mcp add servicenow
```

When prompted for the server details, use:
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

This is a **user-level setting** — you configure it once and it applies to all your Copilot CLI
sessions, in any repository.

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)
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

Restart Claude Desktop.

### VS Code / Cursor / other MCP-compatible editors

Any editor that supports MCP can use `snow mcp` as a server. Consult your editor's MCP
documentation for the exact config format; the command is always `snow mcp`.

## 5. How the skill file works (Copilot CLI only)

This repository contains `skills/servicenow-mcp/SKILL.md`. Load it once via `/skills` in Copilot CLI.

The skill tells Copilot:
- Which MCP tools to call for which tasks (`snow_record_search`, `snow_run_script`, etc.)
- How to run the 5-step reverse engineering workflow
- Which fields to fetch for each artifact type
- How to avoid overloading the context window

> **MCP vs skill — what each one does:**
>
> | | What it does | How it's set up |
> |---|---|---|
> | **MCP server** (`snow mcp`) | Makes `snow` tools callable by the AI | One-time config in `~/.copilot/mcp-config.json` |
> | **Skill file** (`skills/`) | Tells Copilot *when and how* to use those tools | Load once via `/skills` in Copilot CLI |
>
> You need both. The MCP server provides the capability; the skill file provides the intelligence.

## 6. Install MkDocs (for publishing findings)

See [`docs/02-mkdocs.md`](../../docs/02-mkdocs.md) for full instructions. In short:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh   # install uv (once)
uvx mkdocs --version                                # run mkdocs without global install
```

## You're ready

Once `snow login` works, the MCP server is registered, and `mkdocs --version` prints a version,
you have everything.

Continue to [02-methodology.md](02-methodology.md) to start your first investigation.
For documentation setup, see [docs/02-mkdocs.md](../../docs/02-mkdocs.md).
