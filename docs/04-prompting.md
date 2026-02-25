# Prompting Tips for Reverse Engineering

General tips for writing effective AI prompts during a reverse engineering investigation. These apply to any platform — see your platform module under `platforms/` for platform-specific prompt examples.

## Starting an investigation

**Open-ended start:**
> "Reverse engineer the `<feature-name>` functionality. Write the findings to `tutorials/<name>/docs-markdown/`."

This triggers the full 5-step workflow.

**Scoped start:**
> "Reverse engineer the `<feature-name>` feature. Focus on: which data structures it uses, the core logic, and the user-facing workflow."

**Continuing a session:**
> "Continue reverse engineering `<feature>`. We've covered the data model. Now investigate the business logic."

---

## Discovering what exists

```
"How many artifacts match the keyword '<keyword>'?"
"What types of artifacts are there? Give me counts per type."
"List all names — don't fetch content yet, just names."
"Which items are active/enabled?"
```

---

## Fetching content

```
"Fetch the full source for <ArtifactName>."
"Read <ArtifactName> and explain what it does."
"Save all <keyword> items to a file and summarise."
```

---

## Understanding logic

```
"What does <Component>.process() do step by step?"
"Trace what happens when a user triggers <action>."
"Which components call <other-component>?"
```

---

## Writing documentation

```
"Write a markdown tutorial explaining the data model — structures, purpose, key fields, relationships."
"Write an overview explaining what <feature> does and why it exists."
"Document the <Component> API — methods, parameters, return values, usage examples."
"Write a 'recreating from scratch' guide listing all components needed and in what order."
"Create a tutorial section on the user-facing workflow — what actions exist and what they do."
```

---

## Cross-referencing

```
"Which components call <name>?"
"What references <table/schema>?"
"Find all places where '<constant>' appears in logic."
```

---

## Tips for good prompts

**Be specific about scope.** "Explain the approval module" is too broad for one prompt. "Explain the approval lifecycle starting from when a user submits a request" is much better.

**Say where to write output.** Always end documentation prompts with where to save:
> "...write it to `tutorials/<name>/docs-markdown/03-business-logic.md`"

**Ask for examples in docs.** When documenting logic or APIs:
> "Include a code example showing typical usage."

**Name the audience.** Different audiences need different detail levels:
> "Write this for a developer who wants to integrate with this feature, not someone recreating it."

**Iterate.** After a file is written:
> "Review `tutorials/<name>/docs-markdown/02-data-model.md` and identify any gaps — structures or fields we haven't documented yet."
