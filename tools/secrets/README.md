# VTX-MCP-Secrets

`VTX-MCP-Secrets` is the planned local secrets foundation for the VTX MCP tools.

It lets users create and manage secret references without exposing plaintext secrets to Claude or other MCP callers.

## Document Map

```text
README.md              app overview
DESIGN.md              original design notes
ARCHITECTURE.md        process, storage, trust boundary
COMMANDS.md            CLI command contract
MCP-SURFACE.md         MCP tool contract
SECURITY-MODEL.md      threat model and safety rules
IMPLEMENTATION-PLAN.md build phases
examples/              config and metadata examples
```

For the shared concept used by other tools, see:

```text
docs/SECRETS-STORE.md
```

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
init                  # create the local store
set <secretRef>       # prompt for a secret and store it encrypted
list                  # list secret names/metadata only
exists <secretRef>    # return whether a secret exists
delete <secretRef>    # delete a secret
rotate <secretRef>    # replace a secret value
doctor                # verify store health
```

Do not add a plaintext dump command.

## MCP Tools

Planned safe MCP tools:

```text
secrets_list
secrets_exists
secrets_describe
```

Optional later MCP tools:

```text
secrets_delete
secrets_rotate_begin
```

The MCP server should not expose:

```text
secrets_get_plaintext
secrets_export_plaintext
secrets_show
secrets_dump
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
