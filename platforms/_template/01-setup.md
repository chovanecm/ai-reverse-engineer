# Setup — <PLATFORM>

<!-- TODO: Replace <PLATFORM> and all placeholders throughout this file -->

Get the `<PLATFORM>` CLI installed, your instance configured, and the AI assistant connected.

## 1. Install the CLI tool

<!-- TODO: Provide the exact install command -->

```bash
<install command>
```

Verify:
```bash
<tool> --help
```

> **What is `<tool>`?** <!-- TODO: One-sentence description of what this CLI does and why it's needed -->

## 2. Configure your instance

<!-- TODO: Show how to point the tool at a specific instance/environment -->

```bash
<config command> <your-instance>
```

### Managing multiple instances

<!-- TODO: Show how to list, switch, and override instances if applicable -->

```bash
# List configured instances
<tool> list

# Switch default
<tool> use <other-instance>
```

## 3. Test the connection

<!-- TODO: Provide a quick sanity-check command -->

```bash
<tool> login
<tool> <sanity-check-command>
```

If you get a result back, you're connected.

## 4. Register the MCP server (one-time, per machine)

`<tool>` exposes platform tools as an MCP server. Register it once with your AI assistant.

### GitHub Copilot CLI

Edit `~/.copilot/mcp-config.json`:

```json
{
  "mcpServers": {
    "<platform>": {
      "type": "local",
      "command": "<tool>",
      "args": ["mcp"],
      "tools": ["*"]
    }
  }
}
```

<!-- TODO: Adjust args if the MCP server uses a different subcommand -->

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)
or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "<platform>": {
      "command": "<tool>",
      "args": ["mcp"]
    }
  }
}
```

Restart Claude Desktop.

## 5. How the skill file works (Copilot CLI only)

<!-- TODO: Describe the skill file location and what it teaches Copilot -->

This repository contains `skills/<platform>-mcp/SKILL.md`. This file is **automatically loaded**
by GitHub Copilot CLI whenever you work in this directory.

## You're ready

<!-- TODO: Add a final check and link to the next file -->

Once the CLI is installed, your instance is configured, and the MCP server is registered, continue to [02-methodology.md](02-methodology.md).
