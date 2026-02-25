---
name: servicenow-reverse-engineering
description: Methodology for reverse engineering ServiceNow functionality, discovering artifacts, and analyzing business logic.
---

Use this skill when the user asks to reverse engineer, study, or document ServiceNow functionality, or when they provide a keyword to "explore" or "understand".

This skill focuses on the *methodology* and *process*. It assumes the `servicenow-mcp` tools (`snow_record_search`, etc.) are available.

## Reverse engineering workflow

Use this step-by-step process when asked to study or document ServiceNow functionality by keyword.

### Step 1 — Size the problem
```python
snow_record_count(table="sys_metadata", query="GOTO123TEXTQUERY321=<keyword>")
```
This tells you how many artifacts match before downloading anything.

### Step 2 — Discover artifacts
Save the full list of matching artifact IDs and types to disk:
```python
snow_record_search(
    table="sys_metadata",
    query="GOTO123TEXTQUERY321=<keyword>",
    fields="sys_id,sys_class_name",
    limit=200,
    output_file="/tmp/<keyword>_artifacts.json"
)
```
Returns `{"saved_to": "...", "count": N}` — nothing added to context yet.

### Step 3 — Inspect the index
```
view /tmp/<keyword>_artifacts.json
```
Identify which `sys_class_name` types and which `sys_id` values are most relevant. **Do not load everything at once.**

### Step 4 — Fetch artifacts individually (inline, one at a time)
Single-record queries return small payloads that fit safely in context:
```python
snow_record_search(
    table="sys_business_rule",
    query="sys_id=<sys_id>",
    fields="name,script,condition,filter_condition,when,order,active,advanced",
    display_values="values"
)
```
Repeat for each interesting artifact. If there are many of the same type, batch with `sys_idIN<id1>,<id2>,...` and use `output_file`, then read the file.

### Recommended fields per artifact type

| Table | Recommended fields |
|---|---|
| `sys_script_include` | `name,script,description,active` |
| `sys_business_rule` | `name,script,condition,filter_condition,when,order,active,advanced` |
| `sys_ui_action` | `name,script,condition,client_script,hint,active` |
| `sys_ui_script` | `name,script,active` |
| `sysauto_script` | `name,script,active` |
| `sys_ui_page` | `name,html,client_script,processing_script` |
| `sys_transform_entry` | `name,script,condition,active` |
| `sys_ws_operation` | `name,operation_script,active` |

> **Note**: `sys_metadata` text search returns `sys_id` and `sys_class_name` but NOT `name`. Always follow up with a query on the specific child table to get the name and content.

### Step 5 — Write a tutorial
After studying the relevant artifacts, use the repository's template to structure your documentation.

1. Create a new directory for the feature: `cp -r template <feature-name>`
2. Populate `docs-markdown/index.md` with an overview (business logic, implementation patterns).
3. Create additional markdown files in `docs-markdown/` for deep dives (e.g., `data-model.md`, `scripts.md`).
4. Update `mkdocs.yml` to include the new pages in the navigation.

**Do NOT use the `_template` directory.** Use the `template` directory at the root of the repository.

---

## Example prompts

- "List my ServiceNow instances and tell me which one is default."
- "Log in to dev1234.service-now.com and elevate."
- "What fields does the incident table have? Show me reference fields."
- "How many open incidents are there?"
- "Search incident with query active=true, return number and state, limit 20."
- "Export all open incidents to /tmp/open_incidents.json (use output_file)."
- "Save the full schema of cmdb_ci to /tmp/cmdb_ci_schema.json."
- "Run this background script on the default instance: gs.print('hello');"
- "Reverse engineer the 'cbc' functionality — study sys_metadata, download relevant scripts, write a tutorial."
