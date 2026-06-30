# VTX-MCP-Secrets

`VTX-MCP-Secrets` is the planned local secrets foundation for the VTX MCP tools.

It should let users create and manage secret references without exposing plaintext secrets to Claude or other MCP callers.

## Core Idea

Config files should store references, not secret values.

Example:

```json
{
  "provider": "openai",
  "secretRef": "provider/openai/api-key"
}
```

The secret value is stored encrypted in the local secret store.

## Intended Modes

```text
VTX-MCP-Secrets.exe          # MCP stdio mode
VTX-MCP-Secrets.exe -gui     # GUI mode
VTX-MCP-Secrets.exe set ...  # CLI/setup mode
```

## First Commands

Planned CLI commands:

```text
set <secretRef>       # prompt for a secret and store it encrypted
list                  # list secret names/metadata only
exists <secretRef>    # return whether a secret exists
delete <secretRef>    # delete a secret
rotate <secretRef>    # replace a secret value
```

Do not add a plaintext dump command.

## MCP Tools

Planned MCP tools:

```text
secrets_list
secrets_exists
secrets_delete
secrets_rotate
```

The MCP server should not expose:

```text
secrets_get_plaintext
secrets_export_plaintext
secrets_show
```

## Storage Direction

Windows first:

```text
%APPDATA%\VTX\MCP\Secrets\secrets.db
```

Use DPAPI CurrentUser for encrypted values.

Future targets:

```text
Linux:  libsecret / Secret Service, or encrypted local store
macOS:  Keychain
```
