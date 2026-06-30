# Tools

Each folder under `tools/` is a standalone MCP tool area.

## Planned Tools

```text
secrets/    local encrypted secret references and management
ai-broker/  AI provider routing and model registry
rss/        RSS feed fetching, caching, and summaries
ssh/        SSH-based remote administration
snmp/       SNMP discovery and monitoring counter browser
windows/   Windows administration helpers
```

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
