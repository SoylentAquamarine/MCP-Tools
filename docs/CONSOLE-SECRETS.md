# Console-Managed Secrets

This document defines the V1 secrets direction for MCP-Tools.

## Decision

V1 will not build a separate Vault GUI application.

The VTX MCP Console will manage a secrets file directly.

Other MCP tools will reference secrets by name, and the Console will pass the needed secret value to the tool when running a workflow.

## V1 Model

```text
VTX MCP Console
  -> owns secrets.json location
  -> lets user add/edit/remove secrets
  -> passes needed secret values to tools at runtime

MCP tools
  -> do not store their own secrets
  -> receive only what is needed for the requested operation
  -> must not print/log/write returned secrets
```

## Secrets File

The user can choose the secrets file location.

Examples:

```text
C:\Users\<user>\AppData\Roaming\VTX\MCP\secrets.json
E:\VTX-MCP\secrets.json
```

A USB drive is acceptable for V1:

```text
User plugs in USB drive -> Console can use secrets
User removes USB drive -> secrets are unavailable
```

## Plaintext Option

V1 may support plaintext JSON.

Example:

```json
{
  "provider/openai/api-key": "PUT-KEY-HERE",
  "snmp/core-switch-01/community": "public",
  "ssh/lab/password": "PUT-PASSWORD-HERE"
}
```

This must be clearly labeled as insecure.

Required warning:

```text
This secrets file is plaintext. Anyone who can read the file can read the secrets.
Protect the file with OS permissions, removable media, or test-only credentials.
Do not commit this file.
```

## Master Password Option

A later V1/V2 option may encrypt the secrets file with a master password.

Flow:

```text
1. User launches VTX MCP Console.
2. Console asks for master password.
3. Console derives an encryption key from the password.
4. Console decrypts secrets.json in memory.
5. Console passes needed secrets to tools during workflow execution.
6. Console clears decrypted values from memory when locked/closed as best effort.
```

Important rule:

```text
The master password should not be stored.
```

Do not encrypt the master password with an internal app key and pretend that is strong security.

Recommended future crypto direction:

```text
Argon2id or PBKDF2 for key derivation
AES-GCM or XChaCha20-Poly1305 for authenticated encryption
random salt
random nonce per encrypted value or file
```

## Tool Configs

Tool configs should store secret references, not secret values.

Example AI provider config:

```json
{
  "id": "openai",
  "type": "openai",
  "enabled": true,
  "defaultModel": "gpt-4.1-mini",
  "secretRef": "provider/openai/api-key"
}
```

Example SNMP device config:

```json
{
  "deviceId": "core-switch-01",
  "host": "192.168.1.10",
  "version": "v2c",
  "communityRef": "snmp/core-switch-01/community"
}
```

## Runtime Secret Passing

The Console resolves the `secretRef` and passes the value to the tool only for the operation being run.

The tool should not write the value to config, logs, job output, stdout, stderr, or MCP responses.

## Boundary

This model is not perfect security.

It prevents secrets from being scattered across every tool config, but it does not protect against every local attack.

If the user gives an AI unrestricted filesystem or terminal access to the secrets file location, the AI may be able to read it.

That is outside the V1 protection boundary.

## Future Options

Future versions may add:

```text
DPAPI CurrentUser backend
macOS Keychain backend
Linux Secret Service/libsecret backend
encrypted JSON backend
USB/removable secrets profile
per-workflow unlock prompts
audit log for secret use
redaction helpers
```

## Current Direction

Build the tools first.

Use Console-managed `secrets.json` for V1.

Keep the `secretRef` pattern so the backend can be replaced later without rewriting every tool.
