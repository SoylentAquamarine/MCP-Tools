# VTX-MCP-Remote Design

## Scope

VTX-MCP-Remote should provide controlled network remote-access operations through MCP.

V1 protocols:

```text
SSH
Telnet
FTP
```

## Non-Goals for V1

```text
full terminal emulator UI
persistent background agent
credential vault application
unrestricted remote shell
mass automation across unknown hosts
```

## Protocol Adapter Model

Use one tool with protocol adapters.

```text
VTX-MCP-Remote
  core policy engine
  profile loader
  secret resolver interface
  transcript/redaction layer
  ssh adapter
  telnet adapter
  ftp adapter
```

## Config Responsibilities

The tool config should define:

```text
allowed hosts
blocked hosts
allowed protocols
connection profiles
default ports
timeouts
transcript settings
upload/download limits
```

The tool must enforce its own config even when called directly by Claude or another MCP client.

## Console Responsibilities

The Console should define:

```text
which remote commands the AI loop may call
which commands require approval
which commands are disabled
which secrets file is used
which job folder receives transcripts and output
```

## Planned MCP Commands

### remote_test_connection

Tests whether a host/profile can be reached using the selected protocol.

### remote_ssh_exec

Runs a single approved command through SSH and returns structured output.

### remote_ssh_session

Runs a short controlled SSH session script.

### remote_telnet_send

Sends scripted input to a Telnet endpoint and captures output.

Telnet should be disabled by default.

### remote_ftp_list

Lists a remote FTP directory.

### remote_ftp_get

Downloads a remote file into an approved job/output folder.

### remote_ftp_put

Uploads an approved local file from the workspace or job folder.

### remote_ftp_delete

Deletes a remote file.

This should be disabled by default.

## Structured Output

Commands should return JSON with clear fields.

```json
{
  "ok": true,
  "tool": "mcp-remote",
  "command": "remote_ssh_exec",
  "profileId": "lab-linux-01",
  "host": "192.168.1.50",
  "exitCode": 0,
  "stdoutPath": "C:\\AI\\jobs\\job-001\\output\\ssh-stdout.txt",
  "stderrPath": "C:\\AI\\jobs\\job-001\\output\\ssh-stderr.txt",
  "summary": "Command completed."
}
```

Large output should be written to job files and referenced by path.

## Safety Rules

```text
Never return passwords, API keys, private keys, or community strings.
Never log raw secrets.
Reject hosts outside allowedHosts.
Reject protocols disabled in config.
Disable Telnet and FTP by default.
Disable delete operations by default.
Prefer job folders for output.
```

## Future Features

```text
SFTP
SCP
known-host verification
SSH key support
jump host profiles
per-profile command allowlists
interactive session viewer in Console
credential backend abstraction
```
