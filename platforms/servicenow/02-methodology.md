# The 5-Step Reverse Engineering Process — ServiceNow

The general 5-step process is described in the [README Methodology section](../../README.md#methodology).
This file shows how to apply it to ServiceNow specifically, using `snow` CLI tools and the ServiceNow artifact model.

---

## Step 1 — Size the problem

Before fetching anything, find out how much there is.

**Prompt:**
> "How many ServiceNow artifacts match the keyword `<feature-name>`?"

**What Copilot does:**
```python
snow_record_count(table="sys_metadata", query="GOTO123TEXTQUERY321=<keyword>")
```

**Why:** If the count is 50 that's manageable. If it's 3000 you need to narrow the keyword first.

**Example:**
```
Keyword: "approval" → 87 artifacts
```

### Narrowing keywords

If the count is too high, try:
- A more specific term: `"approval_definition"` instead of `"approval"`
- A specific artifact type: ask for counts per `sys_class_name`
- The application name as shown in the ServiceNow app navigator

---

## Step 2 — Discover artifacts

Get the full index of matching artifacts — saved to disk, not into the AI's context.

**Prompt:**
> "Save the full list of `<keyword>` artifact IDs and types to a file."

**What Copilot does:**
```python
snow_record_search(
    table="sys_metadata",
    query="GOTO123TEXTQUERY321=<keyword>",
    fields="sys_id,sys_class_name",
    limit=500,
    output_file="/tmp/<keyword>_artifacts.json"
)
```

Returns only `{"saved_to": "...", "count": N}` — nothing fills the context yet.

---

## Step 3 — Inspect the index

Now look at what you have.

**Prompt:**
> "Read the artifact index and tell me how many of each type there are."

Copilot reads the file and summarises:

```
sys_script_include    32
sys_business_rule     30
sys_ui_action         19
sys_db_object         20
sys_transform_map     15
sys_properties         8
...
```

**Decide what to investigate first.** A good order:
1. `sys_db_object` — custom tables (the data model)
2. `sys_script_include` — business logic
3. `sys_business_rule` — automatic behaviors
4. `sys_ui_action` — user-facing workflow
5. `sys_transform_map` / `sys_transform_entry` — integrations

---

## Step 4 — Fetch selectively

Now read the actual content, one type at a time.

### Fetching a list of names (not content)

Always start by getting names — not scripts — so you can decide which ones matter.

**Prompt:**
> "List the names of all script includes in the artifact index."

**What Copilot does:**
```python
# Get sys_ids of script includes from the saved index
# Then batch-fetch names only (no script content yet)
snow_record_search(
    table="sys_script_include",
    query="sys_idIN<id1>,<id2>,...",
    fields="name,description,active",
    limit=50
)
```

### Fetching a single script

Once you've identified which scripts are important:

**Prompt:**
> "Fetch the full script for `<ScriptIncludeName>`."

```python
snow_record_search(
    table="sys_script_include",
    query="name=<ScriptIncludeName>",
    fields="name,script,description,active",
    display_values="values"
)
```

### Recommended fields per artifact type

| Table | Fields to fetch |
|-------|----------------|
| `sys_script_include` | `name,script,description,active` |
| `sys_business_rule` | `name,script,condition,filter_condition,when,order,active,advanced` |
| `sys_ui_action` | `name,script,condition,client_script,hint,active` |
| `sys_db_object` | `name,label,super_class` |
| `sys_dictionary` | `name,element,column_label,internal_type,reference` |
| `sys_transform_entry` | `name,script,condition,active` |
| `sys_transform_map` | `name,source_table,target_table,active` |
| `sys_properties` | `name,value,description` |

### For large sets — save to disk

When a type has many records (30+ business rules, 42 properties):

**Prompt:**
> "Save all business rules matching `<keyword>` to a file, then summarise what they do."

```python
snow_record_search(
    table="sys_business_rule",
    query="sys_idIN<id1>,<id2>,...",
    fields="name,script,condition,when,active",
    output_file="/tmp/<keyword>_business_rules.json"
)
```

Copilot reads the file progressively, summarising patterns without flooding context.

---

## Step 5 — Write the tutorial

Once you understand a component, have Copilot write it up.

**Prompt:**
> "Based on what we've found, write a markdown tutorial file explaining the data model —
> the tables, their purpose, key fields, and how they relate to each other."

For documentation structure, naming conventions, and MkDocs setup, see [Building docs from findings](../../README.md#building-docs-from-findings).
For general prompting tips when writing docs, see [Prompting the AI](../../README.md#prompting-the-ai).

---

## ServiceNow-specific tips

**Note the `sys_class_name`.** The `sys_metadata` text search matches across all artifact types.
Always confirm which table to query before fetching full records — `sys_script_include` not
`sys_metadata`.

**Cross-reference across artifact types.** Business rules often call script includes. Search
for the script include name in business rule scripts to map dependencies:

> "Which business rules call `<ScriptIncludeName>`?"

**Check active status.** Always filter or note `active=true` — inactive artifacts are noise
when understanding current behavior.

**Use `output_file` for large results.** Any search returning more than ~20 records should
go to a file. The AI reads the file selectively.
