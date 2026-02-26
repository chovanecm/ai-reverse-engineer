# <PLATFORM> — Reverse Engineering with AI

<!-- TODO: Replace <PLATFORM> with your platform name throughout this file -->

Systematically reverse engineer `<PLATFORM>` functionality using `<CLI tool>` and AI assistance (Copilot CLI, Claude Desktop, or VS Code).

## What you can do

<!-- TODO: List 3-4 concrete things users can achieve with this toolkit -->

- Explore any `<PLATFORM>` instance's custom functionality without needing source access or vendor docs
- Discover all artifacts (TODO: list key artifact types) that make a feature work
- Read and analyze the actual code/configuration
- Auto-generate browsable documentation for your team

## Prerequisites

<!-- TODO: Update prerequisites for your platform -->

- Access to a `<PLATFORM>` system/instance (any role that can read configuration/metadata)
- Python 3.8+ and a package manager (`pipx`, `uv`, etc.)
- GitHub Copilot CLI, Claude Desktop, or another MCP-capable AI agent
- 30 minutes for setup; then ready to investigate

## Setup (5 minutes)

<!-- TODO: Replace all <> placeholders with actual commands for your platform -->

### 1. Install the <PLATFORM> CLI

```bash
<install command>
<tool> --help
```

### 2. Configure your <PLATFORM> connection

```bash
<config command> <your-instance-or-host>
# Verify connection
<verify-command>
```

### 3. Register the MCP server

The `<tool>` CLI exposes its tools as an MCP server. Register it once with your AI assistant.

#### GitHub Copilot CLI

Inside a Copilot CLI session:
```
/mcp add <platform>
```

When prompted:
- **Type:** `local`
- **Command:** `<tool>`
- **Args:** `mcp`

Or edit `~/.copilot/mcp-config.json` directly:
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

#### Claude Desktop

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
Then restart Claude Desktop.

#### VS Code / Cursor / Other MCP editors

Consult your editor's MCP documentation. The command is always `<tool> mcp`.

### 4. Load the Copilot skill (Copilot CLI only)

In Copilot CLI:
```
/skills
```

Load `platforms/<platform>/SKILL.md` from this repository.

The skill tells Copilot which `<tool>` commands to call, the 5-step workflow, field recommendations, and context management strategies.

---

## How to use it

Once setup is complete, you can prompt your AI assistant to reverse engineer `<PLATFORM>` features.

### Example: `/plan` command in Copilot CLI

```
/plan Reverse engineer custom functionality XYZ in <PLATFORM> and prepare a tutorial.
Study <artifact-index> to find all relevant artifacts.
Download the actual content and analyze it.
Write comprehensive documentation covering: business logic, data model, data flow, scripts, usage, and integrations.
Save the findings as markdown ready for mkdocs per the template directory.
```

Or simpler:
```
/plan Reverse engineer the <feature> functionality in <PLATFORM>.
```

### If you want to guide the workflow manually, here are example prompts for each step:

<!-- TODO: Adapt these prompts to your platform's terminology and artifacts -->

**Starting an investigation:**
```
"Reverse engineer the <feature-name> functionality. 
Write the findings to tutorials/<name>/docs-markdown/."
```

**Discovering artifacts:**
```
"How many <PLATFORM> artifacts match the keyword '<keyword>'?"
"What types of artifacts are there? (counts per type)"
"List all <artifact-type> names — don't fetch content yet, just names."
```

**Fetching content:**
```
"Fetch the full content of <artifact-name>."
"Read <artifact-name> and explain what it does."
"Fetch all <artifact-type> for <category> and summarize their purpose."
```

**Understanding logic:**
```
"What does <artifact-name>.<function>() do step by step?"
"Which <artifact-type> does <name> call?"
"Trace what happens when <event> occurs."
```

**Cross-referencing:**
```
"Which <artifact-type> call <artifact-name>?"
"What other artifacts reference <name>?"
```

---

## Recommended fields per artifact type

*(your agent already knows this if you loaded the skill, but here it is for reference)*

<!-- TODO: Create a table of your platform's artifact types and important fields -->

| Artifact type | Recommended fields |
|--------------|------------|
| `<type-1>` | `name, script, description, active` |
| `<type-2>` | `name, condition, trigger, active` |
| `<type-3>` | `name, content, enabled` |

---

## <PLATFORM>-specific tips

When reverse engineering, keep these practices in mind:

<!-- TODO: Add 3-4 platform-specific best practices -->

- **TODO: Practice 1** — TODO: Explanation
- **TODO: Practice 2** — TODO: Explanation  
- **TODO: Practice 3** — TODO: Explanation
- **TODO: Practice 4** — TODO: Explanation

For more detailed guidance and examples on these practices, see the "<PLATFORM>-specific tips" section in `SKILL.md`.

---

## Building documentation

If not yet prepared by your AI agent,
refer to the general documentation workflow in the main README how to proceed from your findings to writing markdown tutorials ready for mkdocs.

## What's next

- **First investigation?** Start with a simple feature
- **Want deeper context?** See the [general methodology](../../README.md#methodology) in the main README
- **Adding a new platform?** Copy [`platforms/_template`](../_template) and follow the contributing guide
- **General prompting tips?** See [Prompting the AI](../../README.md#prompting-the-ai)
