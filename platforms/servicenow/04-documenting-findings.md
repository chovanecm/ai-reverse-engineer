# Documenting Your Findings — ServiceNow

How to structure, build, and share the documentation you produce.

> For the full MkDocs setup guide (including `uvx mkdocs build` and `file://` protocol setup), see [`docs/02-mkdocs.md`](../../docs/02-mkdocs.md).

## Project layout

Every reverse engineering project follows the same layout:

```
tutorials/<feature-name>/
├── README.md          — what this documents, how to build the site
├── Makefile           — build/start/clean commands
├── mkdocs.yml         — site config (title, nav, theme)
├── docs-markdown/     — your documentation source (edit these)
│   ├── index.md
│   ├── 01-overview.md
│   └── ...
└── docs-html/         — built site (open index.html in browser)
```

The `docs-markdown/` folder is the only thing you edit. Everything else is infrastructure.

## Start from the template

Copy the project template to start a new investigation:

```bash
cp -r template tutorials/<feature-name>
cd tutorials/<feature-name>
```

Then edit `mkdocs.yml` to set your site name:

```yaml
site_name: My Feature Name
```

## The Makefile

```bash
make build   # convert docs-markdown/ → docs-html/  (MkDocs)
make start   # live preview at http://localhost:8000
make clean   # delete docs-html/
```

`make build` produces flat HTML files in `docs-html/`. Open `docs-html/index.html` directly
in any browser — no web server needed, works offline.

## Writing the markdown files

Use this guide's companion example repository as a style reference.

### Good structure for a feature

```
index.md           — audience guide, contents table, navigation
01-overview.md     — what it is, why it exists, architecture diagram
02-data-model.md   — tables, key fields, ER relationships
03-script-includes.md — API reference with method signatures
04-implementing.md — how to use / integrate
05-behaviors.md    — automatic behaviors, user workflow
06-recreating.md   — how to build from scratch (if relevant)
```

Not every feature needs all files. Start with `index.md` + one overview file, then add detail.

### File format

Plain markdown. No special front matter needed for MkDocs (file names drive the navigation
via `mkdocs.yml`).

Use code blocks for scripts:
````markdown
```javascript
var processor = new MyFeatureProcessor();
processor.init(recordId);
```
````

Use tables for comparing values, listing fields, or showing artifact inventories.

### Naming convention

Number files to control sidebar order: `01-`, `02-`, etc. Use descriptive slugs:
`03-script-includes.md`, not `scripts.md`.

## Updating mkdocs.yml nav

Each time you add a file, add it to the `nav` section in `mkdocs.yml`:

```yaml
nav:
  - Home: index.md
  - Overview: 01-overview.md
  - Data Model: 02-data-model.md
  - Script Includes: 03-script-includes.md
```

The nav label is what appears in the sidebar. Keep it short.

## Links between pages

Use relative filenames:

```markdown
See the [data model](02-data-model.md) for table definitions.
See [implementing a load](04-implementing.md#transform-map-pattern) for the full pattern.
```

MkDocs rewrites these to correct HTML paths automatically.

## Sharing the documentation

**Option 1: Share the folder** — zip up the whole `tutorials/<feature>/` directory.
Colleagues run `make build` to regenerate `docs-html/`, then open `docs-html/index.html`.

**Option 2: Share just `docs-html/`** — zip up only the built `docs-html/` folder.
Recipients open `docs-html/index.html` directly. No tools required.

**Option 3: Host it** — put `docs-html/` on any static file server (GitHub Pages, nginx, S3).
`make start` serves it locally at `http://localhost:8000` for a quick preview.

## Keeping docs current

When you discover more about the feature:

1. Edit the relevant `docs-markdown/*.md` file
2. Run `make build`
3. `docs-html/` is updated

The source of truth is always `docs-markdown/`. The `docs-html/` folder is disposable — regenerate
it any time with `make build`.
