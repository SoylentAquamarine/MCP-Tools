# Tools

Each folder under `tools/` is a standalone MCP tool area.

## Current Tool Skeletons

```text
secrets/   legacy/planning area — secrets are now Console-managed, see docs/CONSOLE-SECRETS.md
ai/        AI provider routing and model registry
terminal/  controlled local command execution
remote/    SSH, Telnet, and command-line FTP to remote hosts
rss/       RSS feed fetching, caching, and summaries
snmp/      SNMP discovery and monitoring counter browser
files/     controlled filesystem access and job folders
git/       safe Git repository inspection and actions
windows/   Windows administration helpers
```

`tools/secrets/` is not a build target for V1. Secrets are managed by the VTX MCP Console via `secrets.json`; see `docs/CONSOLE-SECRETS.md` and `docs/blueprints/TOOL-CONFIG-AND-PERMISSIONS.md`.

`terminal/` and `remote/` are kept separate on purpose: `terminal` is local command execution, `remote` is SSH/Telnet/FTP to other hosts. See `docs/blueprints/MCP-REMOTE-BLUEPRINT.md`.

## First Build Order

```text
1. ai
2. terminal
3. rss or snmp
4. remote
```

`ai` comes first since most workflows need it. `secretRef` resolution comes from the Console, not from a standalone secrets tool.

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
VTX-MCP-Terminal.exe          # MCP stdio mode
VTX-MCP-Terminal.exe <cmd>    # CLI/setup mode, where applicable
```

Tools receive resolved secret values from the Console at runtime; they should not implement their own `-gui` secrets UI. See `docs/MCP-CONSOLE.md`.
