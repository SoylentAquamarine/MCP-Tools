# VTX-MCP-Windows

`VTX-MCP-Windows` is the planned Windows administration helper tool.

## Purpose

Expose safe Windows administration capabilities through MCP.

## Planned MCP Tools

```text
windows_service_status
windows_eventlog_query
windows_process_list
windows_scheduled_tasks
windows_installed_software
windows_network_info
windows_disk_info
```

## Security Direction

Start read-only.

Administrative changes should be explicit and policy-controlled.

## Uses Secrets Store

Future domain or remote admin credentials may use `secretRef`, but the first version should avoid credential handling.

## Status

Skeleton only.
