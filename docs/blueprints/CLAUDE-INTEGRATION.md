# Claude Integration Blueprint

This blueprint defines how Claude Desktop (and any other external MCP client) connects to the MCP-Tools ecosystem.

## Decision

External MCP clients connect to the **Console as a single MCP gateway**, not to secret-using tools directly.

```text
No secrets needed  -> direct connection to the tool is fine
Needs a secret     -> must go through the Console gateway
Tools never pull secrets from anything
```

## Why Gateway

When Claude Desktop calls tools through the Console:

```text
Console permissions apply to Claude Desktop, not just the Console's own AI loop
secretRef resolution works unchanged: the Console is always the process invoking the tool
Tool Activity logs every call from every host in one place
one entry in claude_desktop_config.json instead of one per tool
registry changes never require touching Claude's config
```

The rejected alternative — tools pulling secrets from a running Console — was rejected because:

```text
it turns the Console into a background secrets service with an always-open IPC surface
any local process could ask; authenticating the caller is the same unsolved
  problem that killed the standalone vault design
the Console would hand out secrets for tool calls it never saw, bypassing
  permissions and Tool Activity entirely
```

## The Shim

Claude Desktop spawns MCP servers as stdio child processes. The Console is a long-lived GUI. The bridge is a shim:

```text
MCP-Console.exe gateway
```

The shim is **dumb by design**: it forwards bytes between its stdio and the Console's named pipe, and contains zero MCP logic. All intelligence lives in the running Console. A dumb shim never needs updating when tools or features change.

```text
Claude Desktop
  -> spawns MCP-Console.exe gateway (stdio)
    -> named pipe \\.\pipe\vtx-mcp-console (raw MCP JSON-RPC)
      -> running Console GUI
        -> permissions check -> secretRef resolution -> tool invocation
```

The pipe accepts multiple simultaneous clients; Claude Desktop and the Console's own loop coexist.

Claude Desktop config:

```json
{
  "mcpServers": {
    "vtx-mcp": {
      "command": "C:\\Program Files\\VTX-MCP\\MCP-Console\\MCP-Console.exe",
      "args": ["gateway"]
    }
  }
}
```

## Lifecycle Behavior

```text
Console not running  shim returns a clear MCP error: "VTX MCP Console is not running."
                     No auto-launch in V1.

Console locked       tools still list; non-secret calls work; any call that
                     needs a secretRef fails: "Console is locked - unlock it
                     to use credentials." (see docs/CONSOLE-SECRETS.md)

Console unlocked     everything works, subject to permissions
```

## Tool Naming

Every tool's commands carry its prefix (`remote_ssh_exec`, `files_list`, `snmp_walk`). The gateway aggregates all registered, enabled tools into one MCP surface; the prefix convention guarantees no collisions.

## Permissions and Approval

The gateway uses the same `permissions.json` as the Console's own AI loop. One permission system for all hosts.

```text
allow     the client uses the command freely - no prompt, ever
disabled  never callable
ask       pops a confirmation in the Console GUI - only for commands
          the operator deliberately set to this level
```

Configured = approved. If nothing is set to `ask`, no dialog ever appears.

Future option (do not build yet): per-client permission profiles, so an external client can be granted less than the Console's own loop.

## Jobs

Each external client session gets a job folder under `%ProgramData%\VTX-MCP\MCP-Console\jobs\`, so tools that write large output (remote transcripts, SNMP walks) have a workspace, and Tool Activity records external calls the same as internal ones.
