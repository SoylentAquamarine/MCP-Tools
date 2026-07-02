# Project Structure

The repository is organized so people can quickly find the tool they care about.

## Root

```text
README.md     # project overview
INDEX.md      # current file map
.gitignore    # local/build/runtime exclusions
```

## Documentation

```text
docs/
  PROJECT-STRUCTURE.md
  HUB-SECRETS.md
  MCP-HUB.md

  blueprints/
    MCP-HUB-BLUEPRINT.md
    TOOL-CONFIG-AND-PERMISSIONS.md
    MCP-TERMINAL-BLUEPRINT.md
    CLAUDE-INTEGRATION.md
```

Use `docs/` for project-wide design notes that apply to multiple tools.

Examples:

```text
Hub-managed secrets (current V1 direction, see HUB-SECRETS.md)
the MCP-Hub shell itself (see MCP-HUB.md)
runtime folder layout
cross-tool workflow rules
release/install conventions
```

`docs/blueprints/` holds cross-cutting architecture decisions that span more than one tool or define the Hub shell. Tool-specific design still belongs in that tool's own `DESIGN.md`.

## Tools

```text
tools/
  README.md
  MCP-AI/
  MCP-Console/
  MCP-Terminal/
  MCP-Filesystem/
  MCP-SNMP/
  MCP-RSS/
  MCP-Git/
```

Each tool should follow this pattern:

```text
tools/<tool-name>/
  README.md       # what the tool does and how to use it
  DESIGN.md       # architecture, security model, decisions
  examples/       # config examples
  src/            # source code, once implementation starts
  scripts/        # setup/install helper scripts
```

Tool-specific internals belong inside that tool folder.

Project-wide concepts belong in `docs/`.

## Runtime Layout

Installed binaries should not live beside runtime data.

Windows target layout (see `docs/blueprints/TOOL-CONFIG-AND-PERMISSIONS.md`):

```text
%ProgramFiles%\VTX-MCP\          # installed binaries, one folder per app
  MCP-Hub\
  MCP-AI\
  MCP-<Tool>\

%ProgramData%\VTX-MCP\           # config + runtime data, mirrored per app
  MCP-Hub\
    hub.json                     # Hub shell settings
    tools-registry.json          # where tools are, how the Hub uses them
    permissions.json             # what the Hub's AI loop may call
    secrets.json                 # Hub-managed secrets, see HUB-SECRETS.md
    jobs\ projects\ temp\ output\ logs\   # orchestration workspace
  MCP-AI\
    config.json                  # each tool's own config
  MCP-<Tool>\
    config.json
```

Linux/macOS target layout will be defined later, but should follow the same split:

```text
binaries != config != secrets != work output
```

## Design Rule

MCP tools expose capabilities. The Hub owns the workflow and orchestration (see `docs/MCP-HUB.md`).

Tools should not need to talk directly to each other. They can pass data through configured job/work folders when needed, or through the Hub.
