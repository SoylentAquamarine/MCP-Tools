# VTX-MCP-Secrets MCP Surface

This document defines the MCP-facing tools for `VTX-MCP-Secrets`.

MCP mode is not the full admin surface. It should be intentionally smaller than GUI and CLI mode.

## MCP Server Name

Suggested Claude config name:

```text
vtx-secrets
```

Suggested binary:

```text
VTX-MCP-Secrets.exe
```

## Safe MCP Tools

### `secrets_list`

Lists secret metadata.

Returns:

```json
{
  "secrets": [
    {
      "secretRef": "provider/openai/api-key",
      "scope": "VTX-MCP-AI",
      "backend": "dpapi-current-user",
      "createdAt": "2026-06-30T00:00:00Z",
      "updatedAt": "2026-06-30T00:00:00Z",
      "lastUsedAt": null
    }
  ]
}
```

Does not return ciphertext or plaintext.

### `secrets_exists`

Checks if a secret reference exists.

Input:

```json
{
  "secretRef": "provider/openai/api-key"
}
```

Output:

```json
{
  "exists": true
}
```

### `secrets_describe`

Returns non-secret metadata for one secret.

Input:

```json
{
  "secretRef": "provider/openai/api-key"
}
```

Output:

```json
{
  "secretRef": "provider/openai/api-key",
  "scope": "VTX-MCP-AI",
  "backend": "dpapi-current-user",
  "metadata": {
    "provider": "openai",
    "purpose": "api-key"
  }
}
```

### `secrets_delete`

Deletes a secret reference.

This may be allowed in MCP mode, but should require a config setting:

```json
{
  "allowDeleteFromMcp": false
}
```

Default should be `false`.

### `secrets_rotate_begin`

Optional future tool for starting a rotation flow.

The MCP tool should not accept the new plaintext value directly from Claude.

Better pattern:

```text
MCP asks user to run GUI/CLI rotation.
GUI/CLI collects the secret securely.
```

## Denied MCP Tools

Never expose these in normal release builds:

```text
secrets_get_plaintext
secrets_show_plaintext
secrets_dump_plaintext
secrets_export_plaintext
secrets_decrypt
```

## Secret Use by Other Tools

The Secrets MCP server itself should not become a plaintext broker over MCP.

Other VTX tools should use the shared internal secrets library, not call an MCP plaintext function.

Example:

```text
VTX-MCP-AI -> shared secrets library -> decrypt/use provider/openai/api-key internally
```

Not:

```text
VTX-MCP-AI -> MCP secrets_get_plaintext -> receives key over MCP
```

## Error Rules

Errors must not leak secret material.

Bad:

```text
Failed to parse key sk-abc123...
```

Good:

```text
Failed to use secretRef provider/openai/api-key: provider rejected credentials.
```

## Logging Rules

MCP operations should log:

```text
timestamp
operation
secretRef
success/failure
caller mode
```

Do not log:

```text
plaintext secret
ciphertext
full provider authorization headers
```
