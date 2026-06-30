# VTX-MCP-Terminal

`VTX-MCP-Terminal` is the planned terminal and command execution tool.

It should start safe and policy-based, not as an unrestricted shell.

## Purpose

Expose controlled command execution to MCP callers.

Initial targets:

```text
local PowerShell / cmd on Windows
local shell on Linux/macOS
optional SSH execution later
```

## Uses Secrets Store

Terminal may need secrets for:

```text
SSH password fallback
SSH key passphrases
sudo password only if explicitly configured
```

Preferred model is SSH keys and limited permissions, not stored root passwords.

## Planned MCP Tools

```text
terminal_run
terminal_run_profile
terminal_list_profiles
terminal_workdir_info
terminal_read_output_file
```

## Risk Modes

```text
safe      allowlisted commands only
admin     broader local admin commands
unsafe    unrestricted shell, explicit opt-in
```

Default should be `safe`.
