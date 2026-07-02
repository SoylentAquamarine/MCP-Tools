# MCP-Hub

The MCP-Hub is the planned GUI shell for the MCP-Tools ecosystem.

It is one host application that can configure, run, permission, and orchestrate many standalone MCP tools.

## Core Idea

```text
MCP-Hub
  = GUI shell
  = MCP host/client
  = AI loop/orchestrator
  = tool registry
  = config manager
  = permissions manager
  = job/output viewer
```

The tools remain standalone MCP servers.

The same tools can be used by the MCP-Hub, Claude Desktop, Claude Code, other MCP clients, and a future local AI workstation.

## Main Screens

V1 should start simple.

```text
Chat
Jobs
Tool Registry
Settings
Secrets
Logs
```

## Tool Registry

The registry tracks which MCP tools exist, where their executables are, where their configs live, whether they are enabled, and what commands they expose.

## Settings

Settings are for Hub-level configuration: default AI provider, model, secrets file path, workspace root, job folder, theme, logging, and approval behavior.

## Tool Configs

Each MCP tool should own its own config file.

The Hub should provide a generic config editor.

V1 can be a notepad-style JSON editor.

Later versions can generate forms from each tool's `config.schema.json`.

```text
Basic mode: generated form
Advanced mode: raw JSON editor
```

## Tool Registry vs Tool Config

```text
Tool registry:
  where the tool is and how the Hub uses it

Tool config:
  what the tool itself is allowed/configured to do

Hub permissions:
  what the AI loop is allowed to call
```

## Agent Loop

The Hub owns the orchestration loop.

```text
User request
  -> Hub asks MCP-AI for next step
  -> MCP-AI proposes a tool call
  -> Hub checks permissions
  -> Hub asks for approval if needed
  -> Hub runs the selected MCP tool
  -> Hub saves the result to the job folder
  -> Hub sends result back to MCP-AI
  -> repeat until done
```

Rule:

```text
AI decides next step.
Hub executes tool calls.
MCP tools do the work.
```

## V1 Goal

Build a usable shell first:

```text
chat window
tool registry
tool-call log
job file viewer
settings page
master-password secrets manager (encrypted secrets.json from day 1)
raw JSON config editor
basic command permissions
gateway mode for external MCP clients (Claude Desktop)
```

Do not build a custom GUI for every MCP tool.

Build MCP tools and one Hub shell.
