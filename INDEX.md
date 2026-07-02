# Repository Index

This file is the quick map of the repository. Keep it updated whenever files or folders are added, removed, or renamed.

## Current Files

```text
MCP-Tools/
  README.md
  INDEX.md
  .gitignore

  docs/
    PROJECT-STRUCTURE.md
    CONSOLE-SECRETS.md
    MCP-CONSOLE.md

    blueprints/
      MCP-CONSOLE-BLUEPRINT.md
      TOOL-CONFIG-AND-PERMISSIONS.md
      MCP-REMOTE-BLUEPRINT.md

  tools/
    README.md

    remote/
      README.md
      DESIGN.md
      examples/
        remote-policy.example.json

    ai/
      README.md
      DESIGN.md
      examples/
        providers.example.json

    terminal/
      README.md
      DESIGN.md
      examples/
        terminal-policy.example.json

    rss/
      README.md
      DESIGN.md
      examples/
        feeds.example.json

    snmp/
      README.md
      DESIGN.md
      ARCHITECTURE.md
      MCP-SURFACE.md
      MONITORING-CONSOLE.md
      examples/
        devices.example.json

    files/
      README.md
      DESIGN.md
      examples/
        files-policy.example.json

    git/
      README.md
      DESIGN.md
      examples/
        repositories.example.json

    windows/
      README.md
      DESIGN.md
      examples/
        windows-policy.example.json
```

## Main Areas

### `docs/`
Project-wide documentation and architecture notes.

Use this area to explain concepts shared by multiple tools, such as Console-managed secrets and how other MCP tools use `secretRef` values.

### `tools/`
Each MCP tool gets its own folder. The tool folder should contain its README, design notes, examples, and later source code.

### `tools/remote/`
Planned `VTX-MCP-Remote` tool: SSH, Telnet, and command-line FTP in one tool, kept separate from `tools/terminal/` (local execution only). See `docs/blueprints/MCP-REMOTE-BLUEPRINT.md`.

### `docs/blueprints/`
Cross-cutting architecture blueprints, starting with the VTX MCP Console (GUI shell, agent loop, tool registry, permissions) and how tool configs/permissions are structured.

## Current Tool Folders

```text
tools/ai/
tools/terminal/
tools/remote/
tools/rss/
tools/snmp/
tools/files/
tools/git/
tools/windows/
```

## Maintenance Rule

When a file is added, moved, or deleted, update this `INDEX.md` in the same change.
