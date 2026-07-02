# VTX-MCP-Terminal Design

## Goal

Provide controlled terminal access through MCP.

This tool must be designed carefully because unrestricted terminal access can bypass other safety boundaries, including the Secrets tool.

## Policy First

Every command should be evaluated against policy before execution.

Policy inputs:

```text
command
arguments
working directory
risk mode
allowed profiles
blocked paths
blocked patterns
```

## Default Blocks

Block access to secrets paths by default:

```text
%ProgramData%\VTX-MCP\MCP-Console\secrets.json
the configured secrets file location, wherever the user has placed it (e.g. a USB drive)
```

Block obvious secret dump attempts:

```text
type secrets.json
cat secrets.json
type *.secret
powershell history scraping
```

## First Test with Secrets

Use `VTX-MCP-Terminal` to run safe commands while confirming it cannot dump the Console-managed secrets file.

Expected:

```text
terminal_run whoami                                                allowed
terminal_run dir %ProgramData%\VTX-MCP                             allowed
terminal_run type %ProgramData%\VTX-MCP\MCP-Console\secrets.json   denied
```
