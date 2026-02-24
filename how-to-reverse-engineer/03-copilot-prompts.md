# Copilot Prompts That Work

A reference of prompt patterns that work well for reverse engineering ServiceNow functionality.

## How the servicenow-mcp skill works

When you work in this repository, GitHub Copilot CLI automatically loads the
`.github/skills/servicenow-mcp/SKILL.md` skill. This skill tells Copilot:

- Which `snow` tools are available (and when to use each one)
- How to run the 5-step reverse engineering workflow
- Which fields to fetch for each artifact type
- How to avoid flooding the context window

You don't need to explain any of this — just describe what you want to understand.

---

## Starting an investigation

**Open-ended start:**
> "Reverse engineer the `<feature-name>` functionality in ServiceNow. Write the findings to `tutorials/<name>/markdown/`."

This triggers the full 5-step workflow automatically.

**Scoped start:**
> "Reverse engineer the `<feature-name>` feature. Focus on: which tables it uses, the core script includes, and the user-facing workflow (UI actions)."

**Continuing a session:**
> "Continue reverse engineering `<feature>`. We've covered the data model and script includes. Now investigate the business rules."

---

## Discovering what exists

```
"How many artifacts match the keyword '<keyword>'?"
"What types of artifacts are there? Give me counts per type."
"List all custom tables that match '<keyword>'."
"List all script include names — don't fetch scripts yet, just names."
"Which script includes are active?"
```

---

## Fetching content

```
"Fetch the full script for <ScriptIncludeName>."
"Read <ScriptIncludeName> and explain what it does."
"Fetch all business rules for table <table_name> and summarise their purpose."
"Save all <keyword> properties to a file and explain what each one controls."
"What fields does the <table_name> table have? Show reference fields."
```

---

## Understanding logic

```
"What does <ScriptIncludeName>.processRecord() do step by step?"
"What happens when a load record status changes to 'pending_commit'?"
"Which script includes does <BusinessRuleName> call?"
"Trace what happens when a user clicks 'Finish Review' on a load record."
"What is the difference between a pending approval and a completed approval?"
```

---

## Writing documentation

```
"Write a markdown tutorial explaining the data model — tables, purpose, key fields, relationships."
"Write an overview file explaining what <feature> does and why it exists."
"Document the <ScriptIncludeName> API — methods, parameters, return values, usage examples."
"Write a 'recreating from scratch' guide listing all components needed and in what order to create them."
"Create a tutorial section on the user-facing workflow — what buttons exist and what they do."
```

---

## Cross-referencing

```
"Which business rules call <ScriptIncludeName>?"
"Which transform maps use <ScriptIncludeName>?"
"What other tables reference <table_name>?"
"Find all places where the string '<constant>' appears in scripts."
```

---

## Tips for good prompts

**Be specific about scope.** "Explain the approval module" is too broad for one prompt. "Explain the approval
lifecycle starting from when a user submits a request" is much better.

**Say where to write output.** Always end documentation prompts with where to save:
> "...write it to `tutorials/<name>/markdown/03-script-includes.md`"

**Ask for examples in docs.** When documenting script includes or business rules:
> "Include a code example showing typical usage."

**Name the audience.** Different audiences need different detail levels:
> "Write this for a developer who wants to integrate with this feature, not someone recreating it."

**Iterate.** After a file is written:
> "Review `tutorials/<name>/markdown/02-data-model.md` and identify any gaps — tables or
> fields we haven't documented yet."
