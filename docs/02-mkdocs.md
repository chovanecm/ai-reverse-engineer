# Building Documentation with MkDocs

How to turn your reverse-engineering findings (markdown files) into a browsable static website that works offline and on `file://` URLs.

## Prerequisites

You need [`uv`](https://docs.astral.sh/uv/) — the fast Python package manager. Install it once:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

With `uv` installed, you can run MkDocs without any global install using `uvx`:

```bash
uvx mkdocs --version   # downloads and runs mkdocs in an isolated environment
```

No `pipx install mkdocs` or virtual environment setup needed.

## Start from the template

Copy the investigation template to start a new project:

```bash
cp -r template tutorials/<feature-name>
cd tutorials/<feature-name>
```

## Project layout

Every investigation project follows this layout:

```
tutorials/<feature-name>/
├── README.md           — what this documents and how to build the site
├── Makefile            — build/start/clean commands
├── mkdocs.yml          — site config (title, nav, theme)
├── docs-markdown/      — your documentation source (edit these files)
│   ├── index.md
│   ├── 01-overview.md
│   └── ...
└── docs-html/          — built site (open index.html in any browser)
```

Edit only `docs-markdown/`. Everything else is infrastructure.

## Building the site

```bash
make build   # convert docs-markdown/ → docs-html/
make start   # live preview at http://localhost:8000
make clean   # delete docs-html/
```

`make build` produces flat HTML files in `docs-html/`. Open `docs-html/index.html` directly
in any browser — no web server needed, works offline.

## mkdocs.yml configuration

The template's `mkdocs.yml` is pre-configured for offline use:

```yaml
site_name: Feature Name        # ← change this
docs_dir: docs-markdown
site_dir: docs-html
use_directory_urls: false      # ← required for file:// protocol

theme:
  name: readthedocs
```

`use_directory_urls: false` is critical — it makes MkDocs produce `page.html` files instead of
`page/index.html` directories, so links work when you open the site directly from the filesystem.

## Writing markdown

Use numbered filenames to control sidebar order:

```
docs-markdown/
├── index.md
├── 01-overview.md
├── 02-data-model.md
└── 03-business-logic.md
```

### Adding a page to the nav

Each time you add a file, add it to `mkdocs.yml`:

```yaml
nav:
  - Home: index.md
  - Overview: 01-overview.md
  - Data Model: 02-data-model.md
```

### Cross-page links

Use relative filenames — MkDocs rewrites them to correct HTML paths:

```markdown
See the [data model](02-data-model.md) for table definitions.
See [implementing a load](04-implementing.md#transform-map-pattern) for the full pattern.
```

### Code blocks

````markdown
```javascript
var processor = new MyProcessor();
processor.init(recordId);
```
````

## Sharing the documentation

**Option 1: Share the folder** — zip the whole project directory. Recipients run `make build` then open `docs-html/index.html`.

**Option 2: Share just `docs-html/`** — zip only the built output. Recipients open `docs-html/index.html` directly. No tools required.

**Option 3: Host it** — put `docs-html/` on any static file server (GitHub Pages, S3, nginx).

## Keeping docs current

1. Edit the relevant `docs-markdown/*.md` file
2. Run `make build`
3. `docs-html/` is updated

`docs-markdown/` is the source of truth. `docs-html/` is disposable — regenerate any time.
