# VTX-MCP-Remote

VTX-MCP-Remote is the planned MCP tool for remote access protocols.

V1 scope:

```text
SSH
Telnet
command-line FTP
```

## Purpose

Provide controlled remote access operations through MCP while keeping remote protocol policy separate from local terminal policy.

```text
MCP-Terminal = local commands
MCP-Remote   = remote network sessions and transfers
```

## Planned Capabilities

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
%ProgramData%\VTX-MCP\MCP-Remote\config.json
```

The VTX MCP Console can edit this config directly.

For V1, the Console may use a raw JSON editor.

Later, the tool can ship a JSON Schema so the Console can generate a form.

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

The Console resolves the secret and passes it to the tool only when needed.

## Console Permissions

The Console controls what the AI loop is allowed to call.

Example:

```text
remote_test_connection   allow
remote_ssh_exec          ask
remote_ssh_session       ask
remote_telnet_send       ask_every_time
remote_ftp_put           ask
remote_ftp_delete        disabled
```
