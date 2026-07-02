# MCP Console Blueprint

This blueprint describes the first useful version of the VTX MCP Console.

## Purpose

The Console is the shell around the MCP toolset.

It should replace the need to rely only on Claude Desktop or Claude Code while still keeping the tools compatible with those clients.

## Responsibilities

```text
MCP Console responsibilities:
  manage tools
  manage configs
  manage secrets file location
  run the agent loop
  enforce Console permissions
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

Controls what the Console allows the AI loop to call.

### Secrets

Lets the user choose and edit the Console-managed secrets file.

## Agent Loop

```text
1. User submits task.
2. Console creates job folder.
3. Console sends task, available tools, and current state to MCP-AI.
4. MCP-AI returns either next_tool_call, final_answer, or need_user_input.
5. Console validates requested tool call.
6. Console checks command permissions.
7. Console asks user for approval when required.
8. Console executes the MCP tool.
9. Console stores tool output in the job folder.
10. Console sends the structured result back to MCP-AI.
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
  "toolId": "mcp-files",
  "command": "files_write",
  "args": {
    "path": "C:\\ProgramData\\VTX-MCP\\MCP-Console\\projects\\demo\\index.html",
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
    "C:\\ProgramData\\VTX-MCP\\MCP-Console\\projects\\demo\\index.html"
  ]
}
```

## Safety Layers

Use two layers.

```text
Tool config:
  hard limits enforced by the tool itself

Console permissions:
  limits enforced only when the Console is the host
```

Example:

```text
MCP-Files config only allows the Console workspace under %ProgramData%\VTX-MCP\MCP-Console.
Console permissions require approval for file writes.
```

## Claude Integration

Decided model — see `docs/blueprints/CLAUDE-INTEGRATION.md` for the full spec.

```text
Needs a secret     -> external clients go through the Console gateway
                      (MCP-Console.exe gateway stdio shim -> named pipe -> running Console)
No secrets needed  -> direct connection to the individual tool is fine
Tools never pull secrets; the Console pushes them per-operation
```

Gateway mode keeps Console permissions, secretRef resolution, and Tool Activity logging in effect for external clients.

## V1 Build Target

```text
Go GUI shell (the Console is the only GUI in the project)
chat pane
tool-call log
job file list
settings page
tool registry page
raw JSON config editor
master-password secrets manager (create/unlock/lock/edit, see docs/CONSOLE-SECRETS.md)
basic permissions file
MCP-AI planning loop
gateway mode for external MCP clients (see docs/blueprints/CLAUDE-INTEGRATION.md)
```
