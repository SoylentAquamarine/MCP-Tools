# Tools

Each folder under `tools/` is a standalone MCP tool area.

## Current Tool Skeletons

```text
secrets/   local encrypted secret references and management
ai/        AI provider routing and model registry
terminal/  controlled terminal and command execution
rss/       RSS feed fetching, caching, and summaries
snmp/      SNMP discovery and monitoring counter browser
files/     controlled filesystem access and job folders
git/       safe Git repository inspection and actions
windows/   Windows administration helpers
```

## First Build Order

```text
1. secrets
2. ai
3. terminal
4. rss or snmp
```

`secrets` comes first because several other tools need `secretRef` support.

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
VTX-MCP-Secrets.exe          # MCP stdio mode
VTX-MCP-Secrets.exe -gui     # GUI mode
VTX-MCP-Secrets.exe <cmd>    # CLI/setup mode
```
