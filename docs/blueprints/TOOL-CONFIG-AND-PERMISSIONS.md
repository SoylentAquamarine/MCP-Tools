# Tool Config and Permissions Blueprint

This blueprint defines how the Hub should manage MCP tool configs and command permissions.

## Separation of Concerns

Keep these separate.

```text
Hub settings
  settings for the GUI shell itself

Tool registry
  list of tools, executable paths, config paths, enabled state

Tool config
  each tool's own JSON config

Secrets file
  central secrets.json managed by Hub

Permissions
  what the Hub's AI loop is allowed to call
```

## Folder Layout

Binaries install under `%ProgramFiles%\VTX-MCP\`, one folder per app. Config and runtime data mirror that structure under `%ProgramData%\VTX-MCP\`. Orchestration workspace (jobs, temp, output, logs) belongs to the Hub, since the Hub owns the loop.

```text
%ProgramFiles%\VTX-MCP\
  MCP-Hub\
    MCP-Hub.exe
  MCP-AI\
    MCP-AI.exe
  MCP-Console\
    MCP-Console.exe
  MCP-Terminal\
    MCP-Terminal.exe
  MCP-Filesystem\
    MCP-Filesystem.exe
  MCP-SNMP\
    MCP-SNMP.exe
  MCP-RSS\
    MCP-RSS.exe
  MCP-Git\
    MCP-Git.exe

%ProgramData%\VTX-MCP\
  MCP-Hub\
    hub.json
    tools-registry.json
    permissions.json
    secrets.json          # default location; user may relocate (e.g. USB drive)
    jobs\
    projects\
    temp\
    output\
    logs\
  MCP-AI\
    config.json
  MCP-Console\
    config.json
  MCP-Terminal\
    config.json
  MCP-Filesystem\
    config.json
  MCP-SNMP\
    config.json
  MCP-RSS\
    config.json
  MCP-Git\
    config.json
```

## Tool Registry Entry

```json
{
  "id": "mcp-filesystem",
  "name": "MCP Filesystem",
  "executable": "C:\\Program Files\\VTX-MCP\\MCP-Filesystem\\MCP-Filesystem.exe",
  "configPath": "C:\\ProgramData\\VTX-MCP\\MCP-Filesystem\\config.json",
  "enabled": true,
  "permissionsProfile": "default-filesystem"
}
```

## Tool Config Example

This belongs to the tool.

```json
{
  "allowedRoots": [
    "C:\\ProgramData\\VTX-MCP\\MCP-Hub\\jobs",
    "C:\\ProgramData\\VTX-MCP\\MCP-Hub\\projects"
  ],
  "allowWrites": true,
  "allowDeletes": false
}
```

## Hub Permission Example

This belongs to the Hub.

```json
{
  "toolId": "mcp-filesystem",
  "commands": {
    "filesystem_list": "allow",
    "filesystem_read": "allow",
    "filesystem_write": "ask",
    "filesystem_delete": "disabled"
  }
}
```

## Permission Levels

Start with four levels.

```text
disabled
ask_every_time
allow_session
allow_always
```

Short aliases may be used in JSON:

```text
disabled
ask
session
allow
```

## Risk Labels

Each command should advertise a risk label.

```text
read_only
write
network
admin
secret_using
destructive
```

The Hub can use these labels to choose default permissions.

## Config Editing

Each tool should ship:

```text
config.example.json
config.schema.json
```

The Hub should support:

```text
Basic mode:
  generated form from config.schema.json

Advanced mode:
  raw JSON editor
```

For V1, raw JSON editor is enough.

## Rule

A tool must enforce its own hard limits.

The Hub permission layer is extra control, not the only protection.

If another MCP client calls the tool directly, Hub permissions do not apply.
