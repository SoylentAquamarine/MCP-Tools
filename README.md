# MCP-Tools

Standalone MCP tools for local automation, admin workflows, and AI-assisted operations.

## Goal

This project is built around a simple rule:

> Useful capabilities should be exposed as MCP tools so they can be used by Claude, other MCP clients, and VTX applications.

The tools are intended to be:

- standalone executables
- local-first
- transparent and open source
- safe by default
- usable without a cloud account
- usable by both AI clients and normal applications

## Navigation

Start here:

- `INDEX.md` — current repository file map
- `docs/PROJECT-STRUCTURE.md` — folder structure and layout rules
- `docs/MCP-CONSOLE.md` — the VTX MCP Console GUI shell: tool registry, agent loop, permissions
- `docs/CONSOLE-SECRETS.md` — current (V1) secrets direction: Console-managed `secrets.json`
- `docs/SECRETS-STORE.md` — original shared secrets store concept (superseded by `CONSOLE-SECRETS.md` for V1; kept for the `secretRef` pattern and future encrypted-backend ideas)
- `docs/blueprints/` — cross-cutting architecture blueprints (Console, tool config/permissions, Remote)
- `tools/README.md` — tool folder overview

## Planned Tools

Initial tool ideas:

- `VTX-MCP-AI` — route requests to configured AI providers
- `VTX-MCP-Terminal` — controlled local command execution
- `VTX-MCP-Remote` — SSH, Telnet, and command-line FTP to remote hosts
- `VTX-MCP-RSS` — fetch, cache, and summarize RSS feeds
- `VTX-MCP-SNMP` — SNMP discovery and monitoring counter browser
- `VTX-MCP-Files` — controlled filesystem access and job folders
- `VTX-MCP-Git` — safe Git repository inspection and actions
- `VTX-MCP-Windows` — Windows administration tools

Plus the **VTX MCP Console** — the GUI shell (not a standalone MCP tool) that hosts the agent loop, tool registry, permissions, config editing, and Console-managed secrets. See `docs/MCP-CONSOLE.md`.

## Architecture

Do not build a separate GUI app for every tool. Build standalone MCP tools, and let one Console shell configure, permission, orchestrate, and display them.

```text
VTX-MCP-Tool.exe          # MCP stdio mode
VTX-MCP-Tool.exe <cmd>    # CLI/setup mode, where applicable
```

The intended Windows layout (Console-managed, see `docs/blueprints/TOOL-CONFIG-AND-PERMISSIONS.md`):

```text
C:\AI\
  config\
    console.json
    tools-registry.json
    permissions.json
    secrets.json
    tools\<tool>.json
  jobs\
  projects\
  temp\
  output\
  logs\
```

## Design Rules

- Tools expose capabilities. The Console owns the workflow and orchestration.
- Tools do not need to call each other directly.
- Shared work should pass through a job/work folder.
- Tool configs store `secretRef` values, not raw secrets. The Console resolves `secretRef` and passes the value to a tool only for the operation being run.
- Tools must not print, log, or return secret values.

## Status

Early planning and foundation work.

## License

This project is intended to use the GNU Affero General Public License v3.0.
