# VTX-MCP-Git Design

## Goal

Provide MCP-safe Git inspection and controlled Git write operations.

## Default Mode

Default should be read-only.

Allowed by default:

```text
git status
git diff
git log
git branch --show-current
```

Blocked by default:

```text
git push
git reset --hard
git clean -fd
git commit
git checkout with destructive changes
```

## Repository Registry

Git should operate only on configured repositories.

Example:

```json
{
  "repositories": {
    "mcp-tools": {
      "path": "C:\\git\\MCP-Tools",
      "allowWrites": false
    }
  }
}
```
