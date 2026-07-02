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
  CONSOLE-SECRETS.md
  MCP-CONSOLE.md

  blueprints/
    MCP-CONSOLE-BLUEPRINT.md
    TOOL-CONFIG-AND-PERMISSIONS.md
    MCP-REMOTE-BLUEPRINT.md
```

Use `docs/` for project-wide design notes that apply to multiple tools.

Examples:

```text
Console-managed secrets (current V1 direction, see CONSOLE-SECRETS.md)
the VTX MCP Console shell itself (see MCP-CONSOLE.md)
runtime folder layout
cross-tool workflow rules
release/install conventions
```

`docs/blueprints/` holds cross-cutting architecture decisions that span more than one tool or define the Console shell. Tool-specific design still belongs in that tool's own `DESIGN.md`.

## Tools

```text
tools/
  README.md
  ai/
  terminal/
  remote/
  rss/
  snmp/
  files/
  git/
  windows/
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
  MCP-Console\
  MCP-AI\
  MCP-<Tool>\

%ProgramData%\VTX-MCP\           # config + runtime data, mirrored per app
  MCP-Console\
    console.json                 # Console shell settings
    tools-registry.json          # where tools are, how the Console uses them
    permissions.json             # what the Console's AI loop may call
    secrets.json                 # Console-managed secrets, see CONSOLE-SECRETS.md
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

MCP tools expose capabilities. The Console owns the workflow and orchestration (see `docs/MCP-CONSOLE.md`).

Tools should not need to talk directly to each other. They can pass data through configured job/work folders when needed, or through the Console.
