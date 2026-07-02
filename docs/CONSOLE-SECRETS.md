# Console-Managed Secrets

This document defines the V1 secrets model for MCP-Tools.

## Decision

V1 will not build a separate Vault GUI application.

The VTX MCP Console manages the secrets file directly.

The secrets file is **encrypted with a master password from day 1**. There is no plaintext mode.

Other MCP tools reference secrets by name (`secretRef`), and the Console passes the needed secret value to the tool at runtime.

## V1 Model

```text
VTX MCP Console
  -> owns the secrets file location
  -> encrypts/decrypts the file with a key derived from the master password
  -> lets user add/edit/remove secrets while unlocked
  -> passes needed secret values to tools at runtime

MCP tools
  -> do not store their own secrets
  -> never read the secrets file
  -> receive only what is needed for the requested operation
  -> must not print/log/write received secrets
```

## Master Password (required, V1)

The Console works like a password manager (KeePass model).

Flow:

```text
1. First run: user creates a master password; Console creates the encrypted secrets file.
2. Launch: Console prompts for the master password.
3. Console derives an encryption key from the password (Argon2id).
4. Console decrypts the secrets file in memory only. Disk always stays ciphertext.
5. Console is now "unlocked": secretRef resolution works.
6. Lock (manual, idle timeout, or app close): decrypted values are wiped from memory, best effort.
```

Hard rules:

```text
The master password is never stored anywhere.
There is no recovery. Forgotten password = secrets are unrecoverable.
Do not encrypt the master password with an internal app key and pretend that is strong security.
```

## Lock States

The Console is always in one of two secret states:

```text
locked    ciphertext on disk, nothing usable in memory,
          secretRef resolution fails with a clear "Console is locked" error
unlocked  decrypted map in memory, secretRef resolution works
```

Non-secret operations (and non-secret tools) work in both states.

External MCP clients connected through the Console gateway get the same behavior — see `docs/blueprints/CLAUDE-INTEGRATION.md`.

## Crypto

Pure Go, cross-platform (no DPAPI, no OS keychain dependency):

```text
Argon2id for key derivation (deliberately slow; brute force is expensive)
AES-256-GCM for authenticated encryption
random salt per file
random nonce per encryption
```

## File Format

The file keeps the name `secrets.json`: a JSON envelope with encrypted content.

```json
{
  "version": 1,
  "kdf": "argon2id",
  "kdfParams": { "time": 3, "memoryKiB": 65536, "threads": 4 },
  "salt": "base64...",
  "cipher": "aes-256-gcm",
  "nonce": "base64...",
  "ciphertext": "base64..."
}
```

The decrypted plaintext inside is a flat ref-to-value map:

```json
{
  "provider/openai/api-key": "...",
  "snmp/core-switch-01/community": "...",
  "ssh/lab/password": "..."
}
```

The envelope is versioned so KDF/cipher parameters can be upgraded later without breaking existing files.

## Secrets File Location

The user can choose the secrets file location.

Examples:

```text
%ProgramData%\VTX-MCP\MCP-Console\secrets.json
E:\VTX-MCP\secrets.json
```

A USB drive is a first-class option, and encryption makes it genuinely safe:

```text
User plugs in USB drive -> Console can unlock and use secrets
User removes USB drive  -> secrets are unavailable
Drive lost or stolen    -> file is ciphertext; useless without the master password
```

## Tool Configs

Tool configs store secret references, not secret values.

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

Tools never pull secrets — from the Console, the file, or anywhere else. The Console pushes, per operation, in calls it makes itself.

The tool must not write the value to config, logs, job output, stdout, stderr, or MCP responses.

## Boundary

Encryption at rest protects against the file being read: stolen laptop, leaked backup, lost USB drive, other users on the machine.

It does not protect against malware running as the same user while the Console is unlocked, and it does not help if the user gives an AI unrestricted filesystem or terminal access plus the master password.

Tools like MCP-Terminal and MCP-Files must still block access to the secrets file path by default (defense in depth, even though the file is ciphertext).

## Future Options

Future versions may add:

```text
OS keychain integration for convenience unlock (DPAPI / macOS Keychain / libsecret)
per-workflow unlock prompts
audit log for secret use
redaction helpers
per-client permission profiles for gateway clients
```

## Current Direction

Master-password-encrypted `secrets.json`, managed by the Console, from the first build.

Keep the `secretRef` pattern so storage details can evolve without rewriting any tool.
