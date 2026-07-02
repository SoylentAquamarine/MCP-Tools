# MCP-Console Design

## Goal

Provide controlled local command execution through MCP.

This tool must be designed carefully because unrestricted local command execution can bypass other safety boundaries, including Hub-managed secrets.

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
%ProgramData%\VTX-MCP\MCP-Hub\secrets.json
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

Use `MCP-Console` to run safe commands while confirming it cannot dump the Hub-managed secrets file.

Expected:

```text
console_run whoami                                                allowed
console_run dir %ProgramData%\VTX-MCP                             allowed
console_run type %ProgramData%\VTX-MCP\MCP-Hub\secrets.json   denied
```
