# Repository Index

This file is the quick map of the repository. Keep it updated whenever files or folders are added, removed, or renamed.

## Current Files

```text
MCP-Tools/
  README.md
  INDEX.md
  .gitignore

  docs/
    PROJECT-STRUCTURE.md

  tools/
    README.md
    secrets/
      README.md
      DESIGN.md
      examples/
        claude_desktop_config.example.json
        secret-metadata.example.json
```

## Main Areas

### `docs/`
Project-wide documentation and architecture notes.

### `tools/`
Each MCP tool gets its own folder. The tool folder should contain its README, design notes, examples, and later source code.

### `tools/secrets/`
The first tool area. This will define the shared local secrets model used by future MCP tools.

## Planned Tool Folders

```text
tools/secrets/
tools/ai-broker/
tools/rss/
tools/ssh/
tools/snmp/
tools/windows/
```

## Maintenance Rule

When a file is added, moved, or deleted, update this `INDEX.md` in the same change.
