# MCP Remote Blueprint

This blueprint defines the planned remote-access MCP tool.

## Decision

For V1, SSH, Telnet, and command-line FTP should be one tool.

```text
VTX-MCP-Remote
  SSH
  Telnet
  FTP
```

Do not put these in MCP-Terminal.

```text
MCP-Terminal = local machine command execution
MCP-Remote   = network remote access protocols
```

## Why One Tool

These protocols share the same core concepts.

```text
remote host
port
username
password/key secret reference
allowed host list
session timeout
transcript logging
upload/download policy
credential handling
```

Splitting into three tools too early would duplicate config, permissions, profiles, host rules, logging, and secret handling.

## Protocols

### SSH

Primary protocol.

Used for command execution, scripted collection, remote shell sessions, and later SCP/SFTP if added.

### Telnet

Legacy protocol.

Must be disabled by default.

Only intended for lab devices, old network gear, or legacy systems.

### FTP

Legacy file transfer protocol.

Must be disabled by default.

Only intended for lab or legacy systems.

## Planned Commands

```text
remote_test_connection
remote_ssh_exec
remote_ssh_session
remote_telnet_send
remote_ftp_list
remote_ftp_get
remote_ftp_put
remote_ftp_delete
```

V1 should probably disable delete commands by default.

## Console Permissions

Recommended defaults:

```text
remote_test_connection   allow
remote_ssh_exec          ask
remote_ssh_session       ask
remote_telnet_send       ask_every_time
remote_ftp_list          ask
remote_ftp_get           ask
remote_ftp_put           ask
remote_ftp_delete        disabled
```

## Tool Config

The tool should have its own config file, for example:

```text
%ProgramData%\VTX-MCP\MCP-Remote\config.json
```

That config should contain allowed hosts, protocol settings, and connection profiles.

Secrets should be referenced with `secretRef` values. The Console resolves the secret at runtime.

## Future Split Option

If file transfer grows too large, split later:

```text
MCP-Remote    = SSH/Telnet command/session operations
MCP-Transfer  = FTP/SFTP/SCP file transfer operations
```

Do not split until needed.
