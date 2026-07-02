# Tool Config and Permissions Blueprint

This blueprint defines how the Console should manage MCP tool configs and command permissions.

## Separation of Concerns

Keep these separate.

```text
Console settings
  settings for the GUI shell itself

Tool registry
  list of tools, executable paths, config paths, enabled state

Tool config
  each tool's own JSON config

Secrets file
  central secrets.json managed by Console

Permissions
  what the Console's AI loop is allowed to call
```

## Folder Layout

Binaries install under `%ProgramFiles%\VTX-MCP\`, one folder per app. Config and runtime data mirror that structure under `%ProgramData%\VTX-MCP\`. Orchestration workspace (jobs, temp, output, logs) belongs to the Console, since the Console owns the loop.

```text
%ProgramFiles%\VTX-MCP\
  MCP-Console\
    MCP-Console.exe
  MCP-AI\
    MCP-AI.exe
  MCP-Files\
    MCP-Files.exe
  MCP-Terminal\
  MCP-Remote\
  MCP-SNMP\

%ProgramData%\VTX-MCP\
  MCP-Console\
    console.json
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
  MCP-Files\
    config.json
  MCP-Terminal\
    config.json
  MCP-Remote\
    config.json
  MCP-SNMP\
    config.json
```

## Tool Registry Entry

```json
{
  "id": "mcp-files",
  "name": "MCP Files",
  "executable": "C:\\Program Files\\VTX-MCP\\MCP-Files\\MCP-Files.exe",
  "configPath": "C:\\ProgramData\\VTX-MCP\\MCP-Files\\config.json",
  "enabled": true,
  "permissionsProfile": "default-files"
}
```

## Tool Config Example

This belongs to the tool.

```json
{
  "allowedRoots": [
    "C:\\ProgramData\\VTX-MCP\\MCP-Console\\jobs",
    "C:\\ProgramData\\VTX-MCP\\MCP-Console\\projects"
  ],
  "allowWrites": true,
  "allowDeletes": false
}
```

## Console Permission Example

This belongs to the Console.

```json
{
  "toolId": "mcp-files",
  "commands": {
    "files_list": "allow",
    "files_read": "allow",
    "files_write": "ask",
    "files_delete": "disabled"
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

The Console can use these labels to choose default permissions.

## Config Editing

Each tool should ship:

```text
config.example.json
config.schema.json
```

The Console should support:

```text
Basic mode:
  generated form from config.schema.json

Advanced mode:
  raw JSON editor
```

For V1, raw JSON editor is enough.

## Rule

A tool must enforce its own hard limits.

The Console permission layer is extra control, not the only protection.

If another MCP client calls the tool directly, Console permissions do not apply.
