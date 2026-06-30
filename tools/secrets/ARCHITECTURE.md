# VTX-MCP-Secrets Architecture

## Goal

`VTX-MCP-Secrets` is the local secret management foundation for the MCP-Tools suite.

It provides a consistent way for other tools to reference secrets without placing API keys, passwords, tokens, SNMP strings, or passphrases directly in JSON config files.

## Core Principle

```text
Other tools store secretRef values.
The Secrets tool stores encrypted secret values.
MCP callers can use secrets indirectly.
MCP callers cannot dump plaintext secrets.
```

## Modes

The same executable should support multiple modes:

```text
VTX-MCP-Secrets.exe          # MCP stdio mode
VTX-MCP-Secrets.exe -gui     # GUI mode
VTX-MCP-Secrets.exe <cmd>    # CLI/setup mode
```

## Responsibility Split

### GUI Mode

The GUI is the configuration authority.

It can:

```text
create secrets
rotate secrets
delete secrets
edit metadata
copy secretRef values
manage scopes
view audit history
```

It should not show plaintext secret values by default.

### CLI Mode

The CLI is for setup scripts and power users.

It can:

```text
set secrets by prompting securely
list metadata
check if a secret exists
delete secrets
rotate secrets
validate store access
```

Avoid command options that place secret values directly on the command line.

### MCP Mode

MCP mode is for Claude and other MCP clients.

It can expose safe metadata and management operations.

It must not expose plaintext secret retrieval.

## Storage Model

Recommended Windows default:

```text
%APPDATA%\VTX\MCP\Secrets\secrets.db
```

This keeps secrets per-user by default.

Shared machine config remains under:

```text
C:\ProgramData\VTX\MCP\Config\
```

Installed binaries remain under:

```text
C:\Program Files\VTX\MCP\Secrets\
```

## Secret Record

A secret record should contain:

```text
id              internal ID
secretRef       stable reference name
scope           optional tool or app scope
backend         encryption backend
ciphertext      encrypted value
createdAt       timestamp
updatedAt       timestamp
lastUsedAt      optional timestamp
metadataJson    non-secret metadata
```

Do not store plaintext values.

## Encryption Backends

Initial Windows backend:

```text
dpapi-current-user
```

Future backends:

```text
dpapi-local-machine
linux-secret-service
macos-keychain
file-age-encrypted
external-vault
```

## How Other Tools Use Secrets

Example AI provider config:

```json
{
  "providers": {
    "openai": {
      "type": "openai",
      "defaultModel": "gpt-4.1-mini",
      "secretRef": "provider/openai/api-key"
    }
  }
}
```

`VTX-MCP-AI` reads the config, asks the shared secrets library for `provider/openai/api-key`, uses the decrypted value internally, then discards it.

## Trust Boundary

Claude and MCP clients are outside the trust boundary for secret values.

Trusted code:

```text
VTX-MCP-Secrets internal decrypt function
approved VTX tool secret-use functions
```

Untrusted output surface:

```text
MCP tool responses
logs
stdout/stderr
job workspace files
error messages
```

Plaintext secrets must never cross into the untrusted output surface.
