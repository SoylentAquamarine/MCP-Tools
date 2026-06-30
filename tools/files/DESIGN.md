# VTX-MCP-Files Design

## Goal

Provide controlled filesystem access for MCP workflows.

## Workspace Pattern

Tools should pass intermediate files through job folders:

```text
C:\ProgramData\VTX\MCP\Work\jobs\<jobId>\
  input\
  output\
  temp\
  logs\
  manifest.json
```

## Policy

File operations should be constrained by policy.

Policy should include:

```text
allowed roots
blocked roots
max file size
allowed extensions
write permissions
secret path blocks
```

## Default Blocks

Block secret storage paths by default.

```text
%APPDATA%\VTX\MCP\Secrets\
C:\ProgramData\VTX\MCP\Secrets\
```
