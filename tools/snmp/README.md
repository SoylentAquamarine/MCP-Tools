# VTX-MCP-SNMP

`VTX-MCP-SNMP` is the planned SNMP browser and monitoring discovery tool.

## Purpose

Connect to network devices, walk SNMP data, identify useful counters, and recommend monitoring targets.

## Planned MCP Tools

```text
snmp_test_connection
snmp_get
snmp_walk
snmp_discover_device
snmp_find_interfaces
snmp_find_counters
snmp_recommend_monitors
snmp_export_prometheus
snmp_export_zabbix
```

## Uses Secrets Store

SNMP secrets should be referenced by `secretRef`:

```text
snmp/core-switch-01/community
snmp/core-switch-01/auth-password
snmp/core-switch-01/privacy-password
```

## Status

Skeleton only.
