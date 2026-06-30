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

## Planned Tools

Initial tool ideas:

- `VTX-MCP-Secrets` — local secret references and encrypted secret storage
- `VTX-MCP-AIBroker` — route requests to configured AI providers
- `VTX-MCP-RSS` — fetch, cache, and summarize RSS feeds
- `VTX-MCP-SSH` — SSH-based remote administration tools
- `VTX-MCP-SNMP` — SNMP discovery and monitoring counter browser
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
