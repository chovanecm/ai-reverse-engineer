# Contributing a New Platform Module

This toolkit is designed so that anyone can add support for a new platform by following this guide and submitting a pull request.

## What a platform module is

A platform module is a folder under `platforms/<platform-name>/` containing:

1. **`README.md`** — what the platform is, what the module covers, quick-start
2. **`01-setup.md`** — how to install platform-specific tools, configure credentials, wire up AI
3. **`02-methodology.md`** — the general 5-step process applied to this platform's artifacts and APIs
4. **`03-copilot-prompts.md`** *(optional)* — platform-specific prompt examples (generic tips live in `docs/04-prompting.md`)

Documentation structure and MkDocs setup are covered by the general guides in `docs/` — platform modules should not duplicate them.

Optionally, a platform module may also include a Copilot skill under `skills/<platform>-*/SKILL.md`.

## Step-by-step: adding a new platform

### 1. Copy the template

```bash
cp -r platforms/_template platforms/<your-platform>
```

### 2. Fill in the template files

Each file in `_template/` contains `<!-- TODO: ... -->` comments telling you what to write.

Replace all `<PLATFORM>` placeholders with your platform's name.

### 3. Add a Copilot skill (strongly recommended)

If your platform has an MCP server or CLI tool, create a skill file:

```bash
mkdir -p skills/<platform>-mcp
# create skills/<platform>-mcp/SKILL.md
```

The skill file teaches the AI agent which tools to call and in what order. See [`skills/servicenow-mcp/SKILL.md`](../skills/servicenow-mcp/SKILL.md) as a reference.

### 4. Register your skill in the registry

Add an entry to [`skills/registry.yaml`](../skills/registry.yaml):

```yaml
- id: <platform>-mcp
  name: <Platform> MCP
  file: skills/<platform>-mcp/SKILL.md
  platform: <platform>
  guide: platforms/<platform>/01-setup.md
  prerequisites:
    tools:
      - name: <cli-tool>
        install: "<install command>"
    mcp:
      copilot_cli:
        config_file: ~/.copilot/mcp-config.json
        server:
          type: local
          command: <cli-tool>
          args: [mcp]
```

### 5. Update AGENTS.md

Add a row for your platform to the skills table in [`AGENTS.md`](../AGENTS.md).

### 6. Update README.md

Add your platform to the module table in the root [`README.md`](../README.md).

### 7. Open a pull request

Submit your PR with a title like: `feat: add <Platform> platform module`

Include in the PR description:
- What platform you're adding
- What MCP server or CLI tool it relies on
- A brief description of what the skill enables

## Quality checklist

Before submitting, verify:

- [ ] All `<PLACEHOLDER>` and `<!-- TODO -->` items are replaced
- [ ] `01-setup.md` has working install commands (tested)
- [ ] The skill file is registered in `skills/registry.yaml`
- [ ] `AGENTS.md` has a section for the new platform
- [ ] `README.md` module table is updated
- [ ] File names use lowercase-kebab-case
