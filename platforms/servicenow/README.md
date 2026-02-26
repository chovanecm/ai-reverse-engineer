# ServiceNow — Reverse Engineering with Copilot

Systematically reverse engineer ServiceNow functionality using the `snow` CLI and AI assistance (Copilot CLI, Claude Desktop, or VS Code).

## What you can do

- Explore any ServiceNow instance's custom functionality without needing source access or vendor docs
- Discover all artifacts (tables, scripts, rules, UI actions, transforms) that make a feature work
- Read and analyze JavaScript business logic
- Auto-generate browsable documentation for your team

## Prerequisites

- Access to a ServiceNow instance (any role that can read metadata)
- Python 3.8+ and `pipx` or `uv` or any Python package manager
- GitHub Copilot CLI, Claude Desktop, or another MCP-capable AI agent
- 30 minutes for setup; then ready to investigate

## Setup (5 minutes)

### 1. Install the snow CLI

```bash
pipx install "git+https://github.com/chovanecm/snow-run-python@main"
snow --help
```

### 2. Configure your ServiceNow instance

```bash
snow add --default your-instance.service-now.com
# You'll be prompted for username/password (stored securely in OS keyring)

# Verify connection
snow login
snow record count incident
```

### 3. Register the MCP server

The `snow` CLI exposes its tools as an MCP server. Register it once with your AI assistant.

#### GitHub Copilot CLI

Inside a Copilot CLI session:
```
/mcp add servicenow
```

When prompted:
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

#### Claude Desktop

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
Then restart Claude Desktop.

#### VS Code / Cursor / Other MCP editors

Consult your editor's MCP documentation. The command is always `snow mcp`.

### 4. Load the Copilot skill (Copilot CLI only)

In Copilot CLI:
```
/skills
```

Load `platforms/servicenow/SKILL.md` from this repository.

The skill tells Copilot which `snow` tools to call, the 5-step workflow, field recommendations, and context management strategies.

---

## How to use it

Once setup is complete, you can prompt your AI assistant to reverse engineer ServiceNow features.

### Example: `/plan` command in Copilot CLI

```
/plan Reverse engineer custom functionality XYZ in ServiceNow and prepare a tutorial.
Study sys_metadata to find all relevant artifacts (tables, scripts, rules, transforms).
Download the actual scripts and analyze them.
Write comprehensive documentation covering: business logic, data model, data flow, scripts, business rules, usage, and integrations.
Save the findings as markdown ready for mkdocs per the template directory.
```

Or simpler:
```
/plan Reverse engineer the approval functionality in ServiceNow.
```

### If you want to plan manually, here are example prompts for each step of the workflow:

**Starting an investigation:**
```
"Reverse engineer the <feature-name> functionality. 
Write the findings to tutorials/<name>/docs-markdown/."
```

**Discovering artifacts:**
```
"How many ServiceNow artifacts match the keyword '<keyword>'?"
"What types of artifacts are there? (counts per sys_class_name)"
"List all script include names — don't fetch scripts yet, just names."
```

**Fetching content:**
```
"Fetch the full script for <ScriptIncludeName>."
"Read <ScriptIncludeName> and explain what it does."
"Fetch all business rules for table <table_name> and summarize their purpose."
```

**Understanding logic:**
```
"What does <ScriptIncludeName>.processRecord() do step by step?"
"Which script includes does <BusinessRuleName> call?"
"Trace what happens when a user clicks '<UI Action>' on a record."
```

**Cross-referencing:**
```
"Which business rules call <ScriptIncludeName>?"
"Which transform maps use <ScriptIncludeName>?"
"What other tables reference <table_name>?"
```

---

## Recommended fields per artifact type
*(your agent already knows this if you loaded the skill, but here it is for reference)*

| Table | Recommended fields |
|-------|----------|
| `sys_script_include` | `name,script,description,active` |
| `sys_business_rule` | `name,script,condition,filter_condition,when,order,active,advanced` |
| `sys_ui_action` | `name,script,condition,client_script,hint,active` |
| `sys_db_object` | `name,label,super_class` |
| `sys_dictionary` | `name,element,column_label,internal_type,reference` |
| `sys_ui_script` | `name,script,active` |
| `sysauto_script` | `name,script,active` |
| `sys_ui_page` | `name,html,client_script,processing_script` |
| `sys_transform_map` | `name,source_table,target_table,active` |
| `sys_transform_entry` | `name,script,condition,active` |
| `sys_ws_operation` | `name,operation_script,active` |

---

## ServiceNow-specific tips

When reverse engineering, keep these practices in mind:

- **Always check `sys_class_name`.** The `sys_metadata` text search matches all artifact types. Confirm which specific table to query before fetching full records.
- **Filter by `active=true`.** Inactive artifacts are noise — focus on what's currently running. Include this in your queries.
- **Cross-reference across types.** Business rules call script includes. Search for script include names within business rules to map dependencies.
- **Use `output_file` for large sets.** Any result with 20+ records should go to disk for selective reading.

For more detailed guidance and examples on these practices, see the "ServiceNow-specific tips" section in `SKILL.md`.

---

## Building documentation
If not yet prepared by your AI agent,
refer to the general documentation workflow in the main README how to proceed from your findings to writing markdown tutorials ready for mkdocs.

## What's next

- **First investigation?** Start with a simple feature (e.g., "approval" or "notifications")
- **Want deeper context?** See the [general methodology](../../README.md#methodology) in the main README
- **Adding a new platform?** Copy [`platforms/_template`](../_template) and follow the contributing guide
- **General prompting tips?** See [Prompting the AI](../../README.md#prompting-the-ai)
