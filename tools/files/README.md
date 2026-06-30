# VTX-MCP-Files

`VTX-MCP-Files` is the planned controlled filesystem tool.

## Purpose

Expose safe file operations to MCP callers.

## Planned MCP Tools

```text
files_list_dir
files_read_file
files_write_file
files_copy
files_move
files_delete
files_search
files_create_job_folder
```

## Security Direction

Filesystem access should be policy-based.

Default should allow work folders and configured project folders, not the entire machine.

## Uses Secrets Store

This tool should not normally need secrets, but it must respect secret path blocks.

## Status

Skeleton only.
