# MCP-SNMP Design

## Goal

Provide an advanced SNMP discovery tool that turns raw SNMP walks into useful monitoring recommendations.

## Default Security Direction

Prefer SNMPv3.

Allow SNMPv2c only with warnings.

## Secrets Integration

Device configs store `secretRef` values only.

Example:

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

## First Discovery Target

Start with IF-MIB:

```text
ifName
ifAlias
ifDescr
ifOperStatus
ifAdminStatus
ifHCInOctets
ifHCOutOctets
ifInErrors
ifOutErrors
ifInDiscards
ifOutDiscards
```
