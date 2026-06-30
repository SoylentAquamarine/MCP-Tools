# VTX-MCP-Secrets Implementation Plan

## Phase 1: Documentation and Contracts

Deliverables:

```text
README.md
DESIGN.md
ARCHITECTURE.md
COMMANDS.md
MCP-SURFACE.md
SECURITY-MODEL.md
examples/
```

Goal:

```text
Claude or another coding agent should be able to implement from the docs without guessing the design.
```

## Phase 2: CLI Prototype

Build the first CLI-only version.

Commands:

```text
init
set
list
exists
delete
rotate
doctor
```

Windows first:

```text
DPAPI CurrentUser
SQLite local store
secure prompt for secret entry
```

## Phase 3: Shared Library

Create reusable internal package/library used by future tools.

Responsibilities:

```text
load config
open secret store
decrypt by secretRef
return value only to trusted internal caller
redact unsafe output
write audit events
```

The library should be used by:

```text
VTX-MCP-AI
VTX-MCP-Terminal
VTX-MCP-SNMP
VTX-MCP-RSS if feed credentials are needed
```

## Phase 4: MCP Server Mode

Expose safe MCP tools:

```text
secrets_list
secrets_exists
secrets_describe
```

Optional later:

```text
secrets_delete
secrets_rotate_begin
```

Do not expose plaintext retrieval.

## Phase 5: GUI Mode

Add GUI mode:

```text
VTX-MCP-Secrets.exe -gui
```

GUI functions:

```text
add secret
rotate secret
delete secret
copy secretRef
view metadata
view audit history
run doctor
```

The GUI is the preferred human setup path.

## Phase 6: Cross-Platform

Add backends:

```text
Windows: DPAPI CurrentUser
Linux: Secret Service/libsecret or encrypted local file backend
macOS: Keychain
```

## Open Decisions

```text
language: Go, Rust, or .NET
GUI framework
SQLite schema versioning approach
whether MCP mode can delete secrets by default
whether a separate local secrets service is needed later
```

## Suggested First Implementation Language

Go is the current preferred direction for standalone MCP tools because it cross-compiles well and creates simple single-file binaries.

This is not final.
