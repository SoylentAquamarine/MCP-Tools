# MCP-Console

`MCP-Console` is the planned local command execution tool.

It should start safe and policy-based, not as an unrestricted shell.

Remote sessions (SSH/Telnet/FTP) are not this tool's job — see `tools/MCP-Terminal/`.

## Purpose

Expose controlled local command execution to MCP callers.

Initial targets:

```text
local PowerShell / cmd on Windows
local shell on Linux/macOS
```

## Secrets

Local command execution should rarely need secrets. The one anticipated case:

```text
sudo password, only if explicitly configured
```

If a command needs a credential, the config stores a `secretRef` and the Hub resolves it at runtime (see `docs/HUB-SECRETS.md`). Preferred model is limited permissions, not stored root passwords.

## Planned MCP Tools

```text
console_run
console_run_profile
console_list_profiles
console_workdir_info
console_read_output_file
```

## Risk Modes

```text
safe      allowlisted commands only
admin     broader local admin commands
unsafe    unrestricted shell, explicit opt-in
```

Default should be `safe`.
