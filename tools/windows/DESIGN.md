# VTX-MCP-Windows Design

## Goal

Provide Windows admin visibility and controlled actions through MCP.

## First Version

Read-only tools first:

```text
services
event logs
processes
network config
disk usage
installed software
scheduled tasks
```

## Later Version

Controlled write/admin actions:

```text
restart service
start service
stop service
create scheduled task
run approved PowerShell profile
```

## Policy

Windows actions should use a policy file before any change operation is allowed.

Default:

```text
readOnly: true
allowServiceRestart: false
allowProcessKill: false
allowRegistryWrite: false
```
