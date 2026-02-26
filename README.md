# Reverse Engineering Toolkit

A methodology and tooling guide for systematically reverse engineering software platforms using AI agents.

This toolkit is **platform-agnostic**: shared methodology lives here, and each platform is a self-contained module under `platforms/`.

## Quick start

**I want to reverse engineer ServiceNow** → Go to [`platforms/servicenow/`](platforms/servicenow/)

**I want to understand the methodology** → See [Methodology](#methodology) below

**I want to add a new platform** → See [Contributing](#contributing) and copy [`platforms/_template/`](platforms/_template/)

---

## Platform modules

Each platform module includes installation instructions, setup guidance, and methodology tailored to that platform's artifact model.

| Platform | Module |
|----------|--------|
| ServiceNow | [`platforms/servicenow/`](platforms/servicenow/) |

> Want to add a platform? Copy [`platforms/_template/`](platforms/_template/) and follow the [Contributing](#contributing) steps below.

## Repository layout

```
platforms/
  servicenow/     ← Start here: setup, methodology, prompts, SKILL.md
  _template/      ← Copy this to add a new platform module
template/         ← Copy this to start a new investigation project
README.md         ← You are here: overview, methodology, contributing, building docs
AGENTS.md         ← AI agent bootstrap guide (read if you're an AI agent)
```

---

## Methodology

A universal 5-step process for investigating any software platform. Each platform module applies these steps with platform-specific tooling.

**Core principle: never load everything into the AI's context at once. Index first, read selectively.**

```
Step 1 → Size the problem    (how much is there?)
Step 2 → Discover artifacts  (save an index to disk)
Step 3 → Inspect the index   (what types matter?)
Step 4 → Fetch selectively   (read content one piece at a time)
Step 5 → Document findings   (write a tutorial)
```

### Step 1 — Size the problem

Before fetching anything, count how many artifacts match your target feature.

- If there are 50 matches, you can work through them directly.
- If there are 3,000, narrow the keyword first (more specific term, filter by type, use the exact module name).

### Step 2 — Discover artifacts

Download a minimal index (IDs and types only) to a **file on disk** — not into the AI's context window.

A list of 500 IDs + types is small. The same 500 records with full content would overflow the context. The AI should receive only `{"saved_to": "...", "count": N}`.

### Step 3 — Inspect the index

Read the saved index and summarise what's there: counts per type, names, categories.

Decide what to investigate first. A good general order:
1. Data models / schemas
2. Core business logic (scripts, rules, functions)
3. Automatic behaviors (triggers, event handlers)
4. User-facing actions (buttons, forms, workflows)
5. Integrations (transforms, connectors, APIs)

### Step 4 — Fetch selectively

Read actual content one artifact at a time (or in small batches saved to disk).

- **Names before content** — list names first, decide which are relevant, then fetch only those.
- **One at a time for scripts/code** — a single script fits safely in context; batches of 30 do not.
- **Save large sets to disk** — when a type has many records, save to file and read selectively.

### Step 5 — Document findings

Once you understand a component, write it up as markdown. Good structure for a reverse-engineered feature:

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

### General tips

- **Cross-reference.** Logic in one artifact often references another. Search identifiers across types.
- **Check active/enabled status.** Inactive artifacts are noise — focus on what is currently running.
- **Iterate.** After writing a doc file, ask the agent to review it for gaps before moving on.

---

## Prompting the AI

**Be specific about scope.** "Explain the approval module" is too broad. "Explain the approval lifecycle starting from when a user submits a request" is much better.

**Say where to write output.** End documentation prompts with a path:
> "...write it to `tutorials/<name>/docs-markdown/03-business-logic.md`"

**Name the audience:**
> "Write this for a developer integrating with this feature, not someone recreating it."

**Iterate:**
> "Review `tutorials/<name>/docs-markdown/02-data-model.md` and identify any gaps."

For platform-specific prompt examples, see your platform module (e.g. [`platforms/servicenow/`](platforms/servicenow/)).

---

## Building docs from findings

Use [`template/`](template/) as a starting point for a new investigation project. It produces a browsable static site from your markdown files.

**Prerequisites:** [`uv`](https://docs.astral.sh/uv/) — install once:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Usage:**
```bash
cp -r template tutorials/<feature-name>
cd tutorials/<feature-name>
make build   # converts docs-markdown/ → docs-html/
make start   # live preview at http://localhost:8000
```

Open `docs-html/index.html` in any browser — works offline, no web server needed.

**`mkdocs.yml` is pre-configured for offline use** (`use_directory_urls: false` + `readthedocs` theme). Edit `site_name` and the `nav` section as you add pages.

---

## For AI agents

Read [`AGENTS.md`](AGENTS.md) — it lists all available skills and how to load them.

---

## Contributing

Add a new platform module by copying [`platforms/_template/`](platforms/_template/) and following these steps:

### 1. Copy the template

```bash
cp -r platforms/_template platforms/<your-platform>
```

### 2. Fill in the files

Each file contains `<!-- TODO: ... -->` comments. Replace all `<PLATFORM>` placeholders.

A platform module contains:
1. **`README.md`** — what the platform is, quick-start setup, usage examples, methodology
2. **`SKILL.md`** *(optional)* — Copilot skill file (load via `/skills` in Copilot CLI)

### 3. Update the platform table above

Add a row to the platform table in this README.

### 4. Open a pull request

Title: `feat: add <Platform> platform module`

**Checklist:**
- [ ] All `<PLACEHOLDER>` and `<!-- TODO -->` items replaced in README.md and SKILL.md
- [ ] Credentials and setup tested and working
- [ ] Platform table in root README updated
- [ ] File names use lowercase-kebab-case
