# VTX-MCP-SNMP

`VTX-MCP-SNMP` is the planned SNMP browser, discovery, monitoring, and alert recommendation tool.

## Purpose

Connect to network devices, walk SNMP data, identify useful counters, and recommend monitoring/alerting targets.

The goal is to let Claude browse a device's SNMP interface and turn the raw data into a practical monitoring console model.

## Document Map

```text
README.md                overview
DESIGN.md                original design notes
ARCHITECTURE.md          full architecture and discovery flow
MCP-SURFACE.md           MCP tool contract
MONITORING-CONSOLE.md    monitoring/alerting console model
examples/                device config examples
```

## Planned MCP Tools

```text
snmp_test_connection
snmp_get
snmp_walk
snmp_discover_device
snmp_find_interfaces
snmp_find_counters
snmp_recommend_monitors
snmp_build_console
snmp_export_monitoring_profile
```

## Uses Secrets Store

SNMP secrets should be referenced by `secretRef`:

```text
snmp/core-switch-01/community
snmp/core-switch-01/auth-password
snmp/core-switch-01/privacy-password
```

Plaintext SNMP credentials must not be printed, logged, written to job files, or returned through MCP.

## First Build Target

Start with:

```text
SNMPv2c and SNMPv3 device config
snmp_test_connection
snmp_get
snmp_walk with limits
snmp_discover_device identity/interfaces
snmp_recommend_monitors for IF-MIB
snmp_build_console JSON output
```

## Status

Architecture skeleton created. Implementation not started.
