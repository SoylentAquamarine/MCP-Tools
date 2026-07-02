# MCP-Git

`MCP-Git` is the planned Git repository helper tool.

## Purpose

Expose safe Git operations to MCP callers.

## Planned MCP Tools

```text
git_status
git_diff
git_log
git_branch
git_add
git_commit
git_restore
git_create_branch
```

## Security Direction

Start read-only by default.

Write operations should require explicit configuration.

## Uses Secrets Store

Future remote operations may use:

```text
github/default/token
git/provider/token
```

## Status

Skeleton only.
