# MCP-Terminal

MCP-Terminal is the planned MCP tool for remote access protocols.

V1 scope:

```text
SSH
Telnet
command-line FTP
```

## Purpose

Provide controlled remote access operations through MCP while keeping remote protocol policy separate from local terminal policy.

```text
MCP-Console  = local commands
MCP-Terminal = remote network sessions and transfers
```

## Planned Capabilities

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

## Safety Defaults

```text
SSH enabled by default
Telnet disabled by default
FTP disabled by default
delete operations disabled by default
secret values never returned in MCP output
transcripts redacted where possible
```

## Config

The tool should have its own config file.

Example path:

```text
%ProgramData%\VTX-MCP\MCP-Terminal\config.json
```

The MCP-Hub can edit this config directly.

For V1, the Hub may use a raw JSON editor.

Later, the tool can ship a JSON Schema so the Hub can generate a form.

## Secrets

Remote profiles should use secret references.

Example:

```json
{
  "id": "lab-linux-01",
  "host": "192.168.1.50",
  "protocol": "ssh",
  "username": "stephen",
  "passwordRef": "ssh/lab-linux-01/password"
}
```

The Hub resolves the secret and passes it to the tool only when needed.

## Hub Permissions

The Hub controls what the AI loop is allowed to call.

Example:

```text
terminal_test_connection   allow
terminal_ssh_exec          ask
terminal_ssh_session       ask
terminal_telnet_send       ask_every_time
terminal_ftp_put           ask
terminal_ftp_delete        disabled
```
