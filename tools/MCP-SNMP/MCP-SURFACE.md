# MCP-SNMP MCP Surface

This document defines the MCP tools exposed by `MCP-SNMP`.

## MCP Server Name

Suggested Claude config name:

```text
vtx-snmp
```

Suggested binary:

```text
MCP-SNMP.exe
```

## Tool Design Rule

The MCP surface should expose useful operations, not just raw SNMP.

Raw SNMP operations are useful for testing, but the main value is interpretation.

## Planned MCP Tools

### `snmp_test_connection`

Tests SNMP access to a configured device.

Input:

```json
{
  "deviceId": "core-switch-01"
}
```

Output:

```json
{
  "deviceId": "core-switch-01",
  "success": true,
  "version": "v3",
  "sysName": "core-switch-01",
  "sysObjectID": "1.3.6.1.4.1.example",
  "latencyMs": 43
}
```

### `snmp_get`

Gets one OID from a configured device.

Input:

```json
{
  "deviceId": "core-switch-01",
  "oid": "1.3.6.1.2.1.1.5.0"
}
```

### `snmp_walk`

Walks an OID subtree with safety limits.

Input:

```json
{
  "deviceId": "core-switch-01",
  "oid": "1.3.6.1.2.1.2",
  "maxRows": 1000,
  "timeoutSeconds": 30
}
```

Default behavior should limit excessive output.

### `snmp_discover_device`

Runs structured discovery against a device.

Input:

```json
{
  "deviceId": "core-switch-01",
  "level": "interfaces"
}
```

Levels:

```text
identity
interfaces
counters
full
```

### `snmp_find_interfaces`

Finds interfaces worth monitoring.

Input:

```json
{
  "deviceId": "core-switch-01",
  "includeDown": false,
  "includeLoopbacks": false
}
```

Output includes interface names, aliases, status, speed, and recommended counters.

### `snmp_find_counters`

Finds useful counters for a device or interface.

Input:

```json
{
  "deviceId": "core-switch-01",
  "target": "interfaces"
}
```

Targets:

```text
identity
interfaces
health
environmental
storage
vendor
all
```

### `snmp_recommend_monitors`

Creates a monitoring recommendation set.

Input:

```json
{
  "deviceId": "core-switch-01",
  "profile": "network-switch"
}
```

Output:

```json
{
  "deviceId": "core-switch-01",
  "monitors": [
    {
      "name": "Uplink traffic inbound",
      "oid": "ifHCInOctets.48",
      "type": "counter64",
      "unit": "bytes",
      "pollIntervalSeconds": 60,
      "alertRule": "rate > baseline * 2 for 10m",
      "reason": "Active high-speed uplink interface"
    }
  ]
}
```

### `snmp_build_console`

Builds a simple monitoring/alerting console model from discovery results.

Input:

```json
{
  "deviceId": "core-switch-01",
  "jobId": "optional-job-id",
  "format": "json"
}
```

Output should include dashboard sections:

```text
device summary
availability
interface traffic
interface errors
environmental health
recommended alerts
```

### `snmp_export_monitoring_profile`

Exports recommendations for other systems.

Possible formats:

```text
json
prometheus
zabbix
librenms
markdown
```

Start with JSON and Markdown.

## Secret Handling

MCP inputs and outputs must not include plaintext SNMP credentials.

Allowed:

```text
deviceId
secretRef names
non-secret metadata
```

Blocked:

```text
community string value
SNMPv3 auth password
SNMPv3 privacy password
```

## Output Size Rules

SNMP walks can produce huge output.

Use limits:

```text
maxRows
maxBytes
timeoutSeconds
summaryOnly
writeLargeOutputToJobFile
```

For large discovery output, write to:

```text
%ProgramData%\VTX-MCP\MCP-Hub\jobs\<jobId>\output\
```

Then return the path and summary.
