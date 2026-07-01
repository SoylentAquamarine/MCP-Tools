# SNMP Monitoring and Alerting Console Model

`VTX-MCP-SNMP` should be able to turn SNMP discovery into a monitoring console definition.

This does not require the SNMP tool to run a web server or permanent service.

The tool can produce a console model that another app can render.

## Flow

```text
1. Claude asks VTX-MCP-SNMP to discover a device.
2. VTX-MCP-SNMP walks useful SNMP tables.
3. VTX-MCP-SNMP identifies counters worth monitoring.
4. VTX-MCP-SNMP builds a monitoring profile.
5. VTX-MCP-SNMP writes console JSON/Markdown to a job folder.
6. Claude or VTX Console uses that output.
```

## Console Sections

### Device Summary

```text
hostname
IP address
sysDescr
sysObjectID
uptime
vendor guess
model guess
SNMP version
last discovery time
```

### Availability

Recommended checks:

```text
SNMP response
sysUpTime continuity
interface critical status
```

### Interface Traffic

Recommended counters:

```text
ifHCInOctets
ifHCOutOctets
```

Prefer 64-bit counters when available.

### Interface Health

Recommended counters:

```text
ifOperStatus
ifAdminStatus
ifInErrors
ifOutErrors
ifInDiscards
ifOutDiscards
```

### Environmental Health

Later expansion:

```text
temperature
fan state
power supply state
UPS battery
printer supplies
storage health
```

## Alert Recommendations

Each alert should include:

```text
name
severity
metric source
OID
condition
recommended duration
reason
confidence
```

Example:

```json
{
  "name": "Uplink input errors increasing",
  "severity": "warning",
  "oid": "ifInErrors.48",
  "condition": "rate > 0 for 5m",
  "reason": "Input errors on active uplink usually indicate cabling, duplex, optics, or physical layer issues.",
  "confidence": "high"
}
```

## Console JSON Shape

Example output:

```json
{
  "deviceId": "core-switch-01",
  "generatedAt": "2026-07-01T00:00:00Z",
  "summary": {
    "sysName": "core-switch-01",
    "sysObjectID": "1.3.6.1.4.1.example",
    "vendorGuess": "unknown",
    "profile": "network-switch"
  },
  "sections": [
    {
      "title": "Device Health",
      "widgets": [
        {
          "type": "status",
          "name": "SNMP availability",
          "source": "snmp_test_connection"
        }
      ]
    },
    {
      "title": "Interface Traffic",
      "widgets": [
        {
          "type": "timeseries",
          "name": "Uplink inbound traffic",
          "oid": "ifHCInOctets.48",
          "unit": "bytes_per_second"
        }
      ]
    }
  ],
  "alerts": []
}
```

## Important Boundary

`VTX-MCP-SNMP` should create the model and recommendations.

A separate VTX Console or monitoring system should render the long-running dashboard and perform continuous polling.

The MCP tool is on-demand.
