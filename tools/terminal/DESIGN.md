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
%APPDATA%\VTX\MCP\Secrets\
C:\ProgramData\VTX\MCP\Secrets\
```

Block obvious secret dump attempts:

```text
cat secrets.db
sqlite3 secrets.db
type *.secret
powershell history scraping
```

## First Test with Secrets

Use `VTX-MCP-Terminal` to run safe commands while confirming it cannot dump the Secrets store.

Expected:

```text
terminal_run whoami                       allowed
terminal_run dir C:\ProgramData\VTX\MCP   allowed
terminal_run type %APPDATA%\VTX\MCP\Secrets\secrets.db   denied
```
