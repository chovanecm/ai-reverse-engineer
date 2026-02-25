# Reverse Engineering <PLATFORM> — Step by Step

<!-- TODO: Replace <PLATFORM> and all placeholders throughout this file -->
<!-- TODO: This file adapts the general 5-step methodology to your platform's specific artifacts and APIs -->

The general methodology is described in the [README Methodology section](../../README.md#methodology).
This file shows how to apply it to `<PLATFORM>` specifically.

## Step 0 — Fast track

<!-- TODO: If your platform has a one-prompt shortcut (like a large model that handles everything), describe it here -->

When using `copilot` with a large model (e.g. `claude-opus` or `gemini-3-pro`):

```
Reverse engineer the '<feature>' functionality.
Save an artifact index to a file, then fetch the most relevant scripts and write a tutorial.
```

## Step 1 — Size the problem

<!-- TODO: Describe how to count artifacts on this platform. What search mechanism exists? -->

**Prompt:**
> "How many `<PLATFORM>` artifacts match the keyword `<feature-name>`?"

**What the agent does:**
```
<tool> <count-command> <search-term>
```

**Example:**
```
Keyword: "approval" → 87 artifacts
```

## Step 2 — Discover artifacts

<!-- TODO: Describe how to save an artifact index to disk -->

**Prompt:**
> "Save the full list of `<keyword>` artifact IDs and types to a file."

**What the agent does:**
```
<tool> <search-command> output_file=/tmp/<keyword>_artifacts.json
```

Returns only metadata — nothing fills the context yet.

## Step 3 — Inspect the index

**Prompt:**
> "Read the artifact index and tell me how many of each type there are."

**Decide what to investigate first.** Suggested order for `<PLATFORM>`:

<!-- TODO: List artifact types in a sensible investigation order for your platform -->

1. `<artifact-type-1>` — <!-- describe what this type represents -->
2. `<artifact-type-2>` — <!-- describe what this type represents -->
3. `<artifact-type-3>` — <!-- describe what this type represents -->

## Step 4 — Fetch selectively

<!-- TODO: Show how to fetch names first, then full content for specific artifacts -->

### Names first

**Prompt:**
> "List the names of all `<artifact-type>` in the artifact index — don't fetch content yet."

### Fetch one artifact

**Prompt:**
> "Fetch the full content of `<artifact-name>`."

### Recommended fields per artifact type

<!-- TODO: Fill in the table with artifact types and useful fields for your platform -->

| Artifact type | Fields to fetch |
|--------------|----------------|
| `<type-1>` | `name, script, description, active` |
| `<type-2>` | `name, condition, trigger, active` |

### Save large sets to disk

When a type has many records:

**Prompt:**
> "Save all `<artifact-type>` matching `<keyword>` to a file, then summarise what they do."

## Step 5 — Write the tutorial

**Prompt:**
> "Based on what we've found, write a markdown tutorial explaining the data model —
> the structures, their purpose, key fields, and how they relate to each other."

Good tutorial structure for a `<PLATFORM>` feature:

```
index.md              — overview, audience guide
01-overview.md        — architecture, key concepts, lifecycle
02-data-model.md      — data structures and relationships
03-business-logic.md  — core scripts and rules
04-implementing.md    — how to use or integrate the feature
05-behaviors.md       — automatic behaviors, user workflow
```

See [Building docs from findings](../../README.md#building-docs-from-findings) for how to build a browsable site from these files.

---

## Tips specific to `<PLATFORM>`

<!-- TODO: Add platform-specific tips, gotchas, or useful patterns -->

- <!-- Tip 1 -->
- <!-- Tip 2 -->
- <!-- Tip 3 -->
