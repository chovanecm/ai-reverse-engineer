# <Feature Name> — Documentation

Reverse-engineered documentation for `<feature-name>` on `<instance>`.

## Structure

```
docs-markdown/    ← edit documentation here
docs-html/        ← built site — open docs-html/index.html in browser
mkdocs.yml        ← build configuration
```

## Build

Requires [`uv`](https://docs.astral.sh/uv/) — install once with `curl -LsSf https://astral.sh/uv/install.sh | sh`.

```bash
make build   # generates docs-html/
make start   # live preview at http://localhost:8000
make clean   # remove docs-html/
```

## Contents

| File | Topic |
|------|-------|
| `docs-markdown/index.md` | Overview |
