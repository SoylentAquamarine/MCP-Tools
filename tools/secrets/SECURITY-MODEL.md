# VTX-MCP-Secrets Security Model

## Main Threat

The main threat is accidental or intentional plaintext secret disclosure through:

```text
MCP responses
logs
error messages
job files
command-line history
unrestricted shell tools
```

## Primary Defense

Do not provide any normal release path that returns plaintext secrets.

```text
Secrets can be used.
Secrets cannot be displayed.
```

## Trust Levels

### Trusted

```text
Secrets encryption/decryption library
GUI secret entry flow
CLI secure prompt flow
approved VTX tool internal secret-use path
```

### Less Trusted

```text
Claude
other MCP clients
MCP response text
logs
workspace files
external scripts
```

## Windows Default

Use DPAPI CurrentUser first.

Benefits:

```text
simple Windows-native protection
no separate master password required
secrets are tied to the creating Windows user
works well when Claude runs MCP tools as the same user
```

Limitations:

```text
not portable to another user profile
not portable to another machine
service accounts need separate setup
malware running as same user is still a risk
```

## File Permissions

Default per-user secret path:

```text
%APPDATA%\VTX\MCP\Secrets\
```

Recommended ACL direction:

```text
current user: full control
administrators: inherited/default Windows behavior
other standard users: no access
```

## Redaction

All output paths must run through redaction helpers.

Redact:

```text
API keys
Authorization headers
Bearer tokens
password fields
community strings
SNMP auth/privacy passwords
private key contents
```

## Audit Log

Audit events should include:

```text
timestamp
operation
secretRef
scope
result
process mode
```

Audit events should not include:

```text
plaintext secret
ciphertext
hash of secret unless specifically needed
```

## Dangerous MCP Combinations

Secrets are safer only if the surrounding toolset respects boundaries.

Risky combination:

```text
VTX-MCP-Secrets protects plaintext
but VTX-MCP-Terminal allows unrestricted PowerShell as same user
```

If a caller has unrestricted shell as the same user, it may be able to access local files and invoke local binaries.

Mitigations:

```text
make Terminal policy-based
block access to secret paths by default
block direct calls to secrets binaries that request unsafe operations
log denied attempts
use separate service account later if needed
```

## Release Rule

Release builds must not include plaintext dump tools.

Development-only debug helpers must be disabled by default and clearly marked.
