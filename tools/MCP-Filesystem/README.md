# MCP-Filesystem

`MCP-Filesystem` is the planned controlled filesystem tool.

## Purpose

Expose safe file operations to MCP callers.

## Planned MCP Tools

```text
filesystem_list_dir
filesystem_read_file
filesystem_write_file
filesystem_copy
filesystem_move
filesystem_delete
filesystem_search
filesystem_create_job_folder
```

## Security Direction

Filesystem access should be policy-based.

Default should allow work folders and configured project folders, not the entire machine.

## Uses Secrets Store

This tool should not normally need secrets, but it must respect secret path blocks.

## Status

Skeleton only.
