# Copilot Prompts That Work — ServiceNow

ServiceNow-specific prompt examples for reverse engineering with the `servicenow-mcp` skill.

> **General prompting tips** (scope, audience, iteration, etc.) apply to all platforms — see [Prompting the AI](../../README.md#prompting-the-ai).

## How the servicenow-mcp skill works

Load the `platforms/servicenow/SKILL.md` skill via `/skills` in Copilot CLI. This skill tells Copilot:

- Which `snow` tools are available (and when to use each one)
- How to run the 5-step reverse engineering workflow
- Which fields to fetch for each artifact type
- How to avoid flooding the context window

You don't need to explain any of this — just describe what you want to understand.

---

## ServiceNow-specific prompts

### Starting an investigation

> "Reverse engineer the `<feature-name>` functionality in ServiceNow. Write the findings to `tutorials/<name>/docs-markdown/`."

> "Reverse engineer the `<feature-name>` feature. Focus on: which tables it uses, the core script includes, and the user-facing workflow (UI actions)."

### Discovering artifacts

```
"How many ServiceNow artifacts match the keyword '<keyword>'?"
"What types of artifacts are there? Give me counts per sys_class_name."
"List all custom tables that match '<keyword>'."
"List all script include names — don't fetch scripts yet, just names."
```

### Fetching ServiceNow content

```
"Fetch the full script for <ScriptIncludeName>."
"Read <ScriptIncludeName> and explain what it does."
"Fetch all business rules for table <table_name> and summarise their purpose."
"Save all <keyword> properties to a file and explain what each one controls."
"What fields does the <table_name> table have? Show reference fields."
```

### Understanding ServiceNow logic

```
"What does <ScriptIncludeName>.processRecord() do step by step?"
"What happens when a record status changes to '<value>'?"
"Which script includes does <BusinessRuleName> call?"
"Trace what happens when a user clicks '<UI Action>' on a record."
```

### Cross-referencing

```
"Which business rules call <ScriptIncludeName>?"
"Which transform maps use <ScriptIncludeName>?"
"What other tables reference <table_name>?"
"Find all places where the string '<constant>' appears in scripts."
```
