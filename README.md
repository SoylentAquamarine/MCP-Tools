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
- `docs/SECRETS-STORE.md` — shared secrets store concept and how other tools use it
- `tools/README.md` — tool folder overview
- `tools/secrets/README.md` — Secrets tool app overview
- `tools/secrets/DESIGN.md` — Secrets tool implementation design notes

## Planned Tools

Initial tool ideas:

- `VTX-MCP-Secrets` — local secret references and encrypted secret storage
- `VTX-MCP-AI` — route requests to configured AI providers
- `VTX-MCP-Terminal` — controlled local/remote command execution
- `VTX-MCP-RSS` — fetch, cache, and summarize RSS feeds
- `VTX-MCP-SNMP` — SNMP discovery and monitoring counter browser
- `VTX-MCP-Files` — controlled filesystem access and job folders
- `VTX-MCP-Git` — safe Git repository inspection and actions
- `VTX-MCP-Windows` — Windows administration tools

## Architecture

Each tool should be able to run in multiple modes:

```text
VTX-MCP-Tool.exe          # MCP stdio mode
VTX-MCP-Tool.exe -gui     # optional GUI mode
VTX-MCP-Tool.exe <cmd>    # CLI/setup mode
```

The intended Windows layout is:

```text
C:\Program Files\VTX\MCP\     # installed binaries
C:\ProgramData\VTX\MCP\       # shared config, logs, work folders
%APPDATA%\VTX\MCP\Secrets\    # per-user secrets, where applicable
```

## Design Rules

- Tools expose capabilities.
- The caller owns the workflow.
- Tools do not need to call each other directly.
- Shared work should pass through a job/work folder.
- Config files may contain `secretRef` values.
- Plaintext secrets should not be returned through MCP.

## Status

Early planning and foundation work.

## License

This project is intended to use the GNU Affero General Public License v3.0.
