# Tools

Each folder under `tools/` is a standalone MCP tool area.

## Current Tool Skeletons

Folder name = tool name.

```text
MCP-AI/          AI provider routing and model registry
MCP-Console/     controlled local command execution
MCP-Terminal/    SSH, Telnet, and command-line FTP to remote hosts
MCP-Filesystem/  controlled filesystem access and job folders
MCP-SNMP/        SNMP discovery and monitoring counter browser
MCP-RSS/         RSS feed fetching, caching, and summaries
MCP-Git/         safe Git repository inspection and actions
```

There is no secrets tool. Secrets are managed by the MCP-Hub via `secrets.json`; see `docs/HUB-SECRETS.md` and `docs/blueprints/TOOL-CONFIG-AND-PERMISSIONS.md`.

`MCP-Console/` and `MCP-Terminal/` are kept separate on purpose: Console is local command execution, Terminal is SSH/Telnet/FTP sessions to other hosts. See `docs/blueprints/MCP-TERMINAL-BLUEPRINT.md`.

## First Build Order

```text
1. MCP-AI
2. MCP-Console
3. MCP-RSS or MCP-SNMP
4. MCP-Terminal
```

`MCP-AI` comes first since most workflows need it. `secretRef` resolution comes from the Hub, not from a standalone secrets tool.

## Tool Folder Standard

Each tool should use this layout:

```text
tools/<tool-name>/
  README.md
  DESIGN.md
  examples/
  src/
  scripts/
```

A tool may eventually build to one standalone executable.

Example:

```text
MCP-SNMP.exe          # MCP stdio mode
MCP-SNMP.exe <cmd>    # CLI/setup mode, where applicable
```

Tools receive resolved secret values from the Hub at runtime; they should not implement their own `-gui` secrets UI. See `docs/MCP-HUB.md`.
