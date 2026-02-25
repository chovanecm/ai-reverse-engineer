# General Reverse Engineering Methodology

A universal process for systematically investigating and documenting any software platform. Platform modules (under `platforms/`) apply these same steps with platform-specific tooling.

## The 5-step process

```
Step 1 → Size the problem    (how much is there?)
Step 2 → Discover artifacts  (save an index to disk)
Step 3 → Inspect the index   (what types matter?)
Step 4 → Fetch selectively   (read content one piece at a time)
Step 5 → Document findings   (write a tutorial)
```

The core principle behind all five steps: **never load everything into the AI's context at once**. Index first, read selectively.

---

## Step 1 — Size the problem

Before fetching anything, count how many artifacts match your target feature.

**Why:** If there are 50 matches, you can work through them directly. If there are 3,000 you need a narrower search term first.

**How (generic):** Ask your AI agent to count matching artifacts before downloading any content.

**Narrowing:** If the count is too high, try:
- A more specific keyword or identifier
- Filtering by artifact type / category
- The exact module or application name as the platform labels it

---

## Step 2 — Discover artifacts

Download a minimal index (IDs and types only) to a file — not into the AI's context window.

**Why:** A list of 500 IDs + types is small. The same 500 records with full content would overflow the context.

**Result:** A file on disk containing artifact identifiers. The AI gets only `{"saved_to": "...", "count": N}`.

---

## Step 3 — Inspect the index

Read the saved index and summarise what's there: counts per type, names, categories.

**Decide what to investigate first.** A good general order:
1. Data models / schemas (what data does the feature own?)
2. Core business logic (scripts, rules, functions)
3. Automatic behaviors (triggers, event handlers)
4. User-facing actions (buttons, forms, workflows)
5. Integrations (transforms, connectors, APIs)

---

## Step 4 — Fetch selectively

Read actual content one artifact at a time (or in small batches saved to disk).

**Names before content.** Always list names first, decide which are relevant, then fetch only those.

**One at a time for scripts/code.** A single script or rule at a time fits safely in context. Batches of 30 do not.

**Save large sets to disk.** When a type has many records, save them to a file and have the AI read selectively from it.

---

## Step 5 — Document findings

Once you understand a component, write it up as markdown.

Good documentation structure for a reverse-engineered feature:

```
index.md              — overview, audience guide, navigation
01-overview.md        — architecture, key concepts, lifecycle
02-data-model.md      — data structures, schemas, relationships
03-business-logic.md  — core scripts, rules, functions
04-implementing.md    — how to use or integrate the feature
05-behaviors.md       — automatic behaviors, user workflow
06-recreating.md      — how to rebuild from scratch (optional)
```

Start with `index.md` + one overview file. Add detail incrementally.

---

## General tips

**Don't load everything at once.** It overflows the AI context window. Index → inspect → fetch selectively.

**Cross-reference.** Logic in one artifact often references another. Search for identifiers across artifact types.

**Check active/enabled status.** Inactive or disabled artifacts are noise — focus on what is currently running.

**Iterate.** After writing a documentation file, ask the agent to review it for gaps before moving on.
