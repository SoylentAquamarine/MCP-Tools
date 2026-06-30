# Shared Secrets Store

The shared secrets store is the foundation used by VTX MCP tools that need credentials.

This document explains how the secrets store is used across the toolset. Implementation details for the secrets application belong in `tools/secrets/`.

## Purpose

Many MCP tools need sensitive values:

```text
AI provider API keys
SSH passwords or key passphrases
SNMP community strings
SNMPv3 auth/privacy passwords
GitHub tokens
VMware credentials
```

Those values should not be stored directly in normal JSON config files.

Instead, config files store a `secretRef`.

Example:

```json
{
  "provider": "openai",
  "defaultModel": "gpt-4.1-mini",
  "secretRef": "provider/openai/api-key"
}
```

The actual secret value lives in the encrypted secrets store.

## Rule

```text
Config stores references.
Secrets store values.
Tools use secrets internally.
Claude should not receive plaintext secrets.
```

## How Other Tools Use It

A tool such as `VTX-MCP-AIBroker` reads its provider config:

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

Then the tool asks the local secrets layer for that secret value internally.

The API key is used to call the provider, but it is not returned through MCP output.

## Example Uses

### AI Broker

```text
provider/openai/api-key
provider/groq/api-key
provider/mistral/api-key
provider/gemini/api-key
```

### SSH

```text
ssh/linux-drone/password
ssh/linux-drone/key-passphrase
```

SSH private key files may still live outside the secret store, but passphrases or passwords can be stored as secret references.

### SNMP

```text
snmp/core-switch-01/community
snmp/core-switch-01/auth-password
snmp/core-switch-01/privacy-password
```

### Future Tools

```text
github/default/token
vmware/lab-vcenter/password
windows/domain-admin/password
```

## User Workflow

The normal user flow should be:

```text
1. Open the Secrets GUI or run the Secrets CLI.
2. Add a secret value.
3. Copy or choose the generated secretRef.
4. Use that secretRef in another tool's config.
5. The other tool uses the secret internally when needed.
```

Example:

```powershell
VTX-MCP-Secrets.exe set provider/openai/api-key
```

Then in the AI Broker config:

```json
{
  "secretRef": "provider/openai/api-key"
}
```

## Runtime Layout

Windows target layout:

```text
C:\Program Files\VTX\MCP\Secrets\VTX-MCP-Secrets.exe
C:\ProgramData\VTX\MCP\Config\
C:\ProgramData\VTX\MCP\Logs\
C:\ProgramData\VTX\MCP\Work\
%APPDATA%\VTX\MCP\Secrets\
```

Recommended default:

```text
Per-user secrets protected with DPAPI CurrentUser.
```

That means secrets are decryptable only by the Windows user that created them.

## What MCP Should Expose

Safe operations:

```text
secrets_list
secrets_exists
secrets_delete
secrets_rotate
```

Unsafe operations that should not exist:

```text
secrets_get_plaintext
secrets_export_plaintext
secrets_dump
```

## Responsibility Split

Top-level docs explain the shared secret model.

```text
docs/SECRETS-STORE.md
```

The secrets tool folder explains the actual app design and implementation.

```text
tools/secrets/README.md
tools/secrets/DESIGN.md
```
