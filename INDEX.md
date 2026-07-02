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
    HUB-SECRETS.md
    MCP-HUB.md

    blueprints/
      MCP-HUB-BLUEPRINT.md
      TOOL-CONFIG-AND-PERMISSIONS.md
      MCP-TERMINAL-BLUEPRINT.md
      CLAUDE-INTEGRATION.md

  tools/
    README.md

    MCP-AI/
      README.md
      DESIGN.md
      examples/
        providers.example.json

    MCP-Console/
      README.md
      DESIGN.md
      examples/
        console-policy.example.json

    MCP-Terminal/
      README.md
      DESIGN.md
      examples/
        terminal-policy.example.json

    MCP-Filesystem/
      README.md
      DESIGN.md
      examples/
        filesystem-policy.example.json

    MCP-SNMP/
      README.md
      DESIGN.md
      ARCHITECTURE.md
      MCP-SURFACE.md
      MONITORING-CONSOLE.md
      examples/
        devices.example.json

    MCP-RSS/
      README.md
      DESIGN.md
      examples/
        feeds.example.json

    MCP-Git/
      README.md
      DESIGN.md
      examples/
        repositories.example.json
```

## Main Areas

### `docs/`
Project-wide documentation and architecture notes.

Use this area to explain concepts shared by multiple tools, such as Hub-managed secrets and how other MCP tools use `secretRef` values.

### `docs/blueprints/`
Cross-cutting architecture blueprints: the MCP-Hub (GUI shell, agent loop, tool registry, permissions), tool config/permission structure, the MCP-Terminal tool, and external MCP client integration (Claude Desktop).

### `tools/`
Each MCP tool gets its own folder, named exactly after the tool. The tool folder should contain its README, design notes, examples, and later source code.

`MCP-Console/` is local command execution. `MCP-Terminal/` is SSH/Telnet/FTP sessions to remote hosts. The MCP-Hub GUI shell is not a tool and does not live under `tools/`.

`MCP-SNMP/MONITORING-CONSOLE.md` describes the monitoring console (dashboard) model that MCP-SNMP generates — "console" there is the dashboard sense, unrelated to the MCP-Console tool.

## Current Tool Folders

```text
tools/MCP-AI/
tools/MCP-Console/
tools/MCP-Terminal/
tools/MCP-Filesystem/
tools/MCP-SNMP/
tools/MCP-RSS/
tools/MCP-Git/
```

## Maintenance Rule

When a file is added, moved, or deleted, update this `INDEX.md` in the same change.
