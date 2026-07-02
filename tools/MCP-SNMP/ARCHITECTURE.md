# MCP-SNMP Architecture

## Goal

`MCP-SNMP` lets Claude or another MCP client browse the SNMP interface of a device, discover useful counters, and produce monitoring/alerting recommendations.

It should be more useful than a raw `snmpwalk`.

The tool should answer questions like:

```text
What is this device?
What interfaces matter?
Which counters should I monitor?
Which counters are noise?
What thresholds make sense?
Can you export a monitoring config?
```

## Core Flow

```text
Claude / MCP Client
  -> MCP-SNMP
    -> target device SNMP
      -> discovery cache
      -> recommendations
      -> monitoring profile
      -> optional dashboard/export files
```

## Main Capabilities

```text
connect to SNMP device
SNMP get
SNMP walk
identify device vendor/model/sysObjectID
walk common MIB areas
parse interface tables
identify active interfaces
recommend counters
recommend alerts
export monitoring profiles
write discovery output to job folders
```

## Runtime Mode

This tool should be an on-demand MCP executable, not a Windows service.

```text
MCP-SNMP.exe          # MCP stdio mode
MCP-SNMP.exe -gui     # later GUI/browser mode
MCP-SNMP.exe <cmd>    # CLI/testing mode
```

## Data Layout

Windows target layout:

```text
%ProgramFiles%\VTX-MCP\MCP-SNMP\MCP-SNMP.exe
%ProgramData%\VTX-MCP\MCP-SNMP\snmp-devices.json
%ProgramData%\VTX-MCP\MCP-SNMP\snmp-cache.db
%ProgramData%\VTX-MCP\MCP-SNMP\mibs\
%ProgramData%\VTX-MCP\MCP-Hub\jobs\<jobId>\
```

## Secrets Integration

SNMP credentials should use Hub-managed secrets (see `docs/HUB-SECRETS.md`).

Device configs store secret references only.

```json
{
  "devices": {
    "core-switch-01": {
      "host": "192.168.1.10",
      "version": "v3",
      "username": "monitor",
      "authPasswordRef": "snmp/core-switch-01/auth-password",
      "privacyPasswordRef": "snmp/core-switch-01/privacy-password"
    }
  }
}
```

The tool uses those values internally and must not print or write them to output.

## Discovery Cache

SNMP walks can be large and slow. Cache discovery results.

Cache should include:

```text
device identity
sysObjectID
sysDescr
sysName
interfaces
active counters
last successful discovery
vendor hints
recommended monitor set
```

Do not cache credentials.

## Device Discovery Levels

### Level 1: Identity

```text
sysDescr
sysObjectID
sysUpTime
sysContact
sysName
sysLocation
```

### Level 2: Interfaces

```text
ifIndex
ifDescr
ifName
ifAlias
ifType
ifMtu
ifSpeed
ifHighSpeed
ifAdminStatus
ifOperStatus
```

### Level 3: Interface Counters

```text
ifHCInOctets
ifHCOutOctets
ifInErrors
ifOutErrors
ifInDiscards
ifOutDiscards
```

### Level 4: Device-Specific Areas

Later expansion:

```text
CPU
memory
temperature
fans
power supplies
storage
wireless clients
firewall sessions
UPS battery
printer supplies
```

## Recommendation Engine

The tool should reduce huge SNMP output into useful monitor candidates.

For each counter, include:

```text
name
oid
instance
current value
counter type
unit
recommended polling interval
recommended alert rule
reason
confidence
```

## Alerting Hub Output

The tool should be able to generate a simple monitoring profile that a GUI console can display.

Example sections:

```text
Device Health
Interface Traffic
Interface Errors
Environmental
Availability
```

## Safety Rules

```text
Prefer SNMPv3.
Allow SNMPv2c with warning.
Never log community strings.
Never log SNMPv3 auth/privacy passwords.
Limit discovery to configured hosts or approved one-time targets.
Apply timeout and max-walk limits.
Do not scan arbitrary subnets by default.
```
