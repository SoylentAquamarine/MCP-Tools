# MCP Hub Blueprint

This blueprint describes the first useful version of the MCP-Hub.

## Purpose

The Hub is the shell around the MCP toolset.

It should replace the need to rely only on Claude Desktop or Claude Code while still keeping the tools compatible with those clients.

## Responsibilities

```text
MCP Hub responsibilities:
  manage tools
  manage configs
  manage secrets file location
  run the agent loop
  enforce Hub permissions
  create job folders
  show tool activity
  show generated outputs
```

```text
MCP tool responsibilities:
  expose capabilities
  validate its own config
  enforce its own hard safety limits
  execute requested operation
  return structured output
```

## Pages

### Chat

Primary task window. The user gives the work request here.

### Tool Activity

Shows every proposed and executed tool call.

```text
time
tool
command
arguments summary
approval status
result status
output file path
```

### Jobs

Shows job folders, output files, transcripts, and final reports.

### Tool Registry

Adds/removes tools and discovers command surfaces.

### Tool Configs

Allows editing each tool's JSON config.

Start with a raw JSON editor. Later add JSON Schema forms.

### Permissions

Controls what the Hub allows the AI loop to call.

### Secrets

Lets the user choose and edit the Hub-managed secrets file.

## Agent Loop

```text
1. User submits task.
2. Hub creates job folder.
3. Hub sends task, available tools, and current state to MCP-AI.
4. MCP-AI returns either next_tool_call, final_answer, or need_user_input.
5. Hub validates requested tool call.
6. Hub checks command permissions.
7. Hub asks user for approval when required.
8. Hub executes the MCP tool.
9. Hub stores tool output in the job folder.
10. Hub sends the structured result back to MCP-AI.
11. Loop repeats until final_answer or stop.
```

## Loop States

```text
idle
planning
tool_call_requested
approval_required
tool_running
tool_result_received
analyzing
done
error
cancelled
```

## Tool Call Object

MCP-AI should return a structured next action, not prose-only instructions.

Example:

```json
{
  "type": "tool_call",
  "toolId": "mcp-filesystem",
  "command": "filesystem_write",
  "args": {
    "path": "C:\\ProgramData\\VTX-MCP\\MCP-Hub\\projects\\demo\\index.html",
    "contentFrom": "job:generated-html"
  },
  "reason": "Create the requested website entry point.",
  "risk": "write"
}
```

## Final Answer Object

```json
{
  "type": "final_answer",
  "summary": "The requested files were created.",
  "outputs": [
    "C:\\ProgramData\\VTX-MCP\\MCP-Hub\\projects\\demo\\index.html"
  ]
}
```

## Safety Layers

Use two layers.

```text
Tool config:
  hard limits enforced by the tool itself

Hub permissions:
  limits enforced only when the Hub is the host
```

Example:

```text
MCP-Filesystem config only allows the Hub workspace under %ProgramData%\VTX-MCP\MCP-Hub.
Hub permissions require approval for file writes.
```

## Claude Integration

Decided model — see `docs/blueprints/CLAUDE-INTEGRATION.md` for the full spec.

```text
Needs a secret     -> external clients go through the Hub gateway
                      (MCP-Hub.exe gateway stdio shim -> named pipe -> running Hub)
No secrets needed  -> direct connection to the individual tool is fine
Tools never pull secrets; the Hub pushes them per-operation
```

Gateway mode keeps Hub permissions, secretRef resolution, and Tool Activity logging in effect for external clients.

## V1 Build Target

```text
Go GUI shell (the Hub is the only GUI in the project)
chat pane
tool-call log
job file list
settings page
tool registry page
raw JSON config editor
master-password secrets manager (create/unlock/lock/edit, see docs/HUB-SECRETS.md)
basic permissions file
MCP-AI planning loop
gateway mode for external MCP clients (see docs/blueprints/CLAUDE-INTEGRATION.md)
```
