---
name: <platform>-reverse-engineering
description: Methodology for reverse engineering <PLATFORM> functionality, discovering artifacts, and analyzing logic.
---

<!-- TODO: Replace <platform> and <PLATFORM> throughout this file -->

Use this skill when the user asks to reverse engineer, study, or document `<PLATFORM>` functionality, or when they provide a keyword to "explore" or "understand".

This skill focuses on the *methodology* and *process*. It assumes the `<PLATFORM>`-mcp tools are available.

## Reverse engineering workflow

Use this step-by-step process when asked to study or document `<PLATFORM>` functionality by keyword.

### Step 1 — Size the problem

<!-- TODO: Show how to count artifacts matching a keyword on your platform -->

```python
<tool_call>(query="<keyword>", count_only=True)
```

This tells you how many artifacts match before downloading anything.

### Step 2 — Discover artifacts

Save the full list of matching artifact IDs and types to disk:

<!-- TODO: Show how to fetch index of IDs and types (minimal data) to a file -->

```python
<tool_call>(
    query="<keyword>",
    fields="id,type",
    output_file="/tmp/<keyword>_artifacts.json"
)
```

Returns `{"saved_to": "...", "count": N}` — nothing added to context yet.

### Step 3 — Inspect the index

```
view /tmp/<keyword>_artifacts.json
```

Identify which `type` values and which `id` values are most relevant. **Do not load everything at once.**

### Step 4 — Fetch artifacts individually (inline, one at a time)

Single-record queries return small payloads that fit safely in context:

<!-- TODO: Show how to fetch full content for a single artifact -->

```python
<tool_call>(
    id="<id>",
    fields="name,content,description,status"
)
```

Repeat for each interesting artifact. If the tool supports batching, use `output_file` for large sets.

### Recommended fields per artifact type

<!-- TODO: Fill in the table with your platform's artifact types and important fields -->

| Artifact type | Recommended fields |
|---|---|
| `<type-1>` | `name, content, description, active` |
| `<type-2>` | `name, condition, trigger, active` |
| `<type-3>` | `name, definition, enabled` |

### Step 5 — Write a tutorial

After studying the relevant artifacts, use the repository's template to structure your documentation.

1. Create a new directory for the feature: `cp -r template <feature-name>`
2. Populate `docs-markdown/index.md` with an overview (business logic, implementation patterns).
3. Create additional markdown files in `docs-markdown/` for deep dives (e.g., `data-model.md`, `logic.md`).
4. Update `mkdocs.yml` to include the new pages in the navigation.

**Do NOT use the `_template` directory.** Use the `template` directory at the root of the repository.

---

## <PLATFORM>-specific tips for investigation

When reverse engineering, always keep these practices in mind:

<!-- TODO: Add 4-5 platform-specific investigation tips with explanations and examples -->

### Tip 1: TODO

<!-- TODO: Explain what to do, why, and when -->

Example:
```
<example code or command>
```

### Tip 2: TODO

<!-- TODO: Explain what to do, why, and when -->

### Tip 3: TODO

<!-- TODO: Explain what to do, why, and when -->

### Don't load everything at once

Always use the 5-step workflow: size → discover → inspect → fetch selectively. Index first, then read only what matters.

---

## Example prompts

<!-- TODO: Add real example prompts for your platform -->

- "List my <PLATFORM> instances."
- "What artifacts match the keyword '<feature>'?"
- "Fetch the full content of <artifact-name>."
- "Reverse engineer the '<feature>' functionality — study the artifacts, download relevant content, write a tutorial."


