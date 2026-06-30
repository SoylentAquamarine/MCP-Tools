# VTX-MCP-Secrets Commands

This document defines the first CLI command surface for `VTX-MCP-Secrets`.

The CLI is for humans, setup scripts, and testing. MCP mode should expose a smaller and safer surface.

## Command Pattern

```text
VTX-MCP-Secrets.exe <command> [arguments]
```

## First Commands

### Initialize Store

```powershell
VTX-MCP-Secrets.exe init
```

Creates the local secrets store if it does not exist.

Expected behavior:

```text
create secrets folder
create SQLite database
enable WAL mode
create schema
write first audit entry
```

### Set Secret

```powershell
VTX-MCP-Secrets.exe set provider/openai/api-key
```

Prompts for the secret value securely.

Do not support this as the default pattern:

```powershell
VTX-MCP-Secrets.exe set provider/openai/api-key --value sk-plaintext
```

Command-line history can leak values.

### List Secrets

```powershell
VTX-MCP-Secrets.exe list
```

Shows metadata only.

Example output:

```text
provider/openai/api-key    scope=VTX-MCP-AI      backend=dpapi-current-user
ssh/lab/key-passphrase     scope=VTX-MCP-Terminal backend=dpapi-current-user
```

### Secret Exists

```powershell
VTX-MCP-Secrets.exe exists provider/openai/api-key
```

Returns success if the secret exists.

### Delete Secret

```powershell
VTX-MCP-Secrets.exe delete provider/openai/api-key
```

Deletes the encrypted secret record after confirmation.

### Rotate Secret

```powershell
VTX-MCP-Secrets.exe rotate provider/openai/api-key
```

Prompts for a replacement value and updates the encrypted ciphertext.

### Validate Store

```powershell
VTX-MCP-Secrets.exe doctor
```

Checks:

```text
store path exists
database opens
schema version is current
encryption backend works
current user can decrypt a test value
```

## Future Commands

```text
backup-metadata       export non-secret metadata
import-metadata       import non-secret references/scopes
scope-list            list scopes
scope-set             assign scope metadata
backend-list          list available backends
migrate-backend       move selected secrets to another backend
```

## Explicitly Denied Commands

Do not implement these by default:

```text
show <secretRef>
get <secretRef>
dump
export-plaintext
print-plaintext
```

If a diagnostic plaintext command is ever added for development, it must be hidden behind a compile-time debug flag and never shipped in release binaries.
