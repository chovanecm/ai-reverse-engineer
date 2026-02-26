# Reverse Engineering Toolkit

Platform-agnostic toolkit for producing structured reverse-engineering documentation with AI.

## Start here (action flow)

1. **Choose a platform module**
   - ServiceNow: [`platforms/servicenow/README.md`](platforms/servicenow/README.md)
2. **Follow that module's setup and investigation flow**
   - Each module is the single source of truth for its platform steps and tooling
3. **Write findings into the docs template**
   - Copy [`template/`](template/) and fill `docs-markdown/`
4. **Build a browsable site**
   - Run `make build` in your copied tutorial folder

If you are using Copilot skills, see [`AGENTS.md`](AGENTS.md).

---

## Repository layout

```
platforms/
  servicenow/     ← ServiceNow module (single action-flow guide + SKILL.md)
  _template/      ← starter for adding another platform module
template/         ← starter for publishing investigation docs with MkDocs
README.md         ← global flow and repository structure
AGENTS.md         ← skill loading and agent bootstrap
```

---

## Build docs from findings

Create a tutorial workspace from [`template/`](template/):

Prerequisite: install [`uv`](https://docs.astral.sh/uv/) once (`curl -LsSf https://astral.sh/uv/install.sh | sh`).

```bash
cp -r template tutorials/<feature-name>
cd tutorials/<feature-name>
make build   # generates docs-html/
make start   # live preview at http://localhost:8000
```

Then open `docs-html/index.html`.

---

## Add a new platform module

```bash
cp -r platforms/_template platforms/<your-platform>
```

Fill in the template files, then add the new platform row in this README and in [`AGENTS.md`](AGENTS.md).
