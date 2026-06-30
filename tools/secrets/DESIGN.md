# VTX-MCP-Secrets Design

## Purpose

Provide a small local secrets layer for standalone MCP tools.

The goal is not to replace enterprise vaults. The goal is to give local MCP tools a safe way to reference and use secrets without putting API keys, passwords, community strings, or tokens in JSON files.

## Security Rule

Secrets may be used by trusted tools.

Secrets should not be revealed through MCP output.

## Data Model

A secret has:

```text
secretRef      stable name used by configs
scope          optional tool/application scope
backend        storage/encryption backend
createdAt      timestamp
updatedAt      timestamp
lastUsedAt     optional timestamp
metadata       non-secret labels/details
ciphertext     encrypted value
```

Example secret references:

```text
provider/openai/api-key
provider/groq/api-key
ssh/linux-drone/password
snmp/core-switch-01/auth-password
snmp/core-switch-01/privacy-password
```

## Windows Storage

Initial target:

```text
%APPDATA%\VTX\MCP\Secrets\secrets.db
```

Recommended encryption:

```text
DPAPI CurrentUser
```

That means the same Windows user who created the secret can decrypt it.

## Access Model

GUI mode:

```text
read/write secrets
read/write config
human setup and rotation
```

MCP/headless mode:

```text
read/use secrets internally
list metadata
check existence
no plaintext return
```

## Denied Operations

These should not exist as MCP tools:

```text
get plaintext secret
export plaintext secret
show secret value
dump all secrets
```

If requested, the tool should return a denial message and log the attempt.

## Logging

Log metadata only.

Allowed log data:

```text
secretRef
operation
success/failure
timestamp
caller mode
```

Never log secret values.

## Concurrency

Multiple processes may use the same secret store:

```text
VTX-MCP-Secrets.exe -gui
VTX-MCP-AIBroker.exe
VTX-MCP-SSH.exe
```

Use SQLite WAL mode or another safe concurrent storage approach.

Rules:

```text
multiple readers allowed
single writer at a time
no long-lived plaintext secret cache
```

## Later Hardening

Possible future improvements:

```text
signed VTX tool allowlist
separate local secrets service
per-tool secret scopes
approval prompts for first use
secret usage audit report
machine-scope option for service accounts
```
