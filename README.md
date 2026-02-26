# Reverse Engineering Toolkit

A methodology and tooling guide for systematically reverse engineering software platforms using AI agents.

This toolkit is **platform-agnostic**: shared methodology lives here in `README.md`, and each platform is a self-contained module under `platforms/`.

## Start here (new users)

If your goal is to quickly produce structured reverse-engineering docs:

1. **Pick a platform module** (for ServiceNow use [`platforms/servicenow/`](platforms/servicenow/)).
2. **Install and configure tools** using the module's `01-setup.md`.
3. **Run the investigation workflow** from the module's `02-methodology.md`.
4. **Publish your findings as browsable docs** using [`template/`](template/).

Fast path for ServiceNow:
- Setup: [`platforms/servicenow/01-setup.md`](platforms/servicenow/01-setup.md)
- Investigation workflow: [`platforms/servicenow/02-methodology.md`](platforms/servicenow/02-methodology.md)
- Prompt examples: [`platforms/servicenow/03-copilot-prompts.md`](platforms/servicenow/03-copilot-prompts.md)
- Build docs site: [`template/README.md`](template/README.md)

If you're extending this repository with another platform, copy [`platforms/_template/`](platforms/_template/).

## Platform modules

| Platform | Module | Skill |
|----------|--------|-------|
| ServiceNow | [`platforms/servicenow/`](platforms/servicenow/) | [`platforms/servicenow/SKILL.md`](platforms/servicenow/SKILL.md) |

> Want to add a platform? See [Contributing](#contributing) below and copy [`platforms/_template/`](platforms/_template/).

## Repository layout

```
platforms/
  servicenow/     ← ServiceNow guide: setup, methodology, prompts, SKILL.md
  _template/      ← Copy this to add a new platform module
template/         ← Copy this to start a new investigation project
README.md         ← You are here: methodology, contributing, building docs
AGENTS.md         ← AI agent bootstrap guide (read this if you're an AI agent)
```

> **Two templates, different purposes:**
> - `platforms/_template/` — skeleton for *adding a new platform* to this repo
> - `template/` — starting point for *a new investigation project* (creates browsable docs from findings)

---

## Methodology

A universal 5-step process for investigating and documenting any software platform. Each platform module applies these steps with platform-specific tooling.

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

For platform-specific prompt examples, see the platform module (e.g. [`platforms/servicenow/03-copilot-prompts.md`](platforms/servicenow/03-copilot-prompts.md)).

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
1. **`README.md`** — what the platform is, quick-start
2. **`01-setup.md`** — install tools, configure credentials, wire up AI
3. **`02-methodology.md`** — the 5-step process applied to this platform's artifact model
4. **`03-copilot-prompts.md`** *(optional)* — platform-specific prompt examples
5. **`SKILL.md`** *(optional)* — Copilot skill file (load via `/skills` in Copilot CLI)

### 3. Update AGENTS.md and README.md

Add a row to the skills table in [`AGENTS.md`](AGENTS.md) and to the platform table above.

### 4. Open a pull request

Title: `feat: add <Platform> platform module`

**Checklist:**
- [ ] All `<PLACEHOLDER>` and `<!-- TODO -->` items replaced
- [ ] `01-setup.md` install commands tested and working
- [ ] `AGENTS.md` skills table updated
- [ ] `README.md` platform table updated
- [ ] File names use lowercase-kebab-case
