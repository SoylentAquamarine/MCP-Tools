# MCP Remote Blueprint

This blueprint defines the planned remote-access MCP tool.

## Decision

For V1, SSH, Telnet, and command-line FTP should be one tool.

```text
MCP-Terminal
  SSH
  Telnet
  FTP
```

Do not put these in MCP-Console.

```text
MCP-Console  = local machine command execution
MCP-Terminal = network remote access protocols
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
terminal_test_connection
terminal_ssh_exec
terminal_ssh_session
terminal_telnet_send
terminal_ftp_list
terminal_ftp_get
terminal_ftp_put
terminal_ftp_delete
```

V1 should probably disable delete commands by default.

## Hub Permissions

Recommended defaults:

```text
terminal_test_connection   allow
terminal_ssh_exec          ask
terminal_ssh_session       ask
terminal_telnet_send       ask_every_time
terminal_ftp_list          ask
terminal_ftp_get           ask
terminal_ftp_put           ask
terminal_ftp_delete        disabled
```

## Tool Config

The tool should have its own config file, for example:

```text
%ProgramData%\VTX-MCP\MCP-Terminal\config.json
```

That config should contain allowed hosts, protocol settings, and connection profiles.

Secrets should be referenced with `secretRef` values. The Hub resolves the secret at runtime.

## Future Split Option

If file transfer grows too large, split later:

```text
MCP-Terminal    = SSH/Telnet command/session operations
MCP-Transfer  = FTP/SFTP/SCP file transfer operations
```

Do not split until needed.
