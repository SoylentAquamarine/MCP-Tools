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
    SECRETS-STORE.md

  tools/
    README.md

    secrets/
      README.md
      DESIGN.md
      ARCHITECTURE.md
      COMMANDS.md
      MCP-SURFACE.md
      SECURITY-MODEL.md
      IMPLEMENTATION-PLAN.md
      examples/
        claude_desktop_config.example.json
        secret-metadata.example.json
        secrets-store-config.example.json

    ai/
      README.md
      DESIGN.md
      examples/
        providers.example.json

    terminal/
      README.md
      DESIGN.md
      examples/
        terminal-policy.example.json

    rss/
      README.md
      DESIGN.md
      examples/
        feeds.example.json

    snmp/
      README.md
      DESIGN.md
      examples/
        devices.example.json

    files/
      README.md
      DESIGN.md
      examples/
        files-policy.example.json

    git/
      README.md
      DESIGN.md
      examples/
        repositories.example.json

    windows/
      README.md
      DESIGN.md
      examples/
        windows-policy.example.json
```

## Main Areas

### `docs/`
Project-wide documentation and architecture notes.

Use this area to explain concepts shared by multiple tools, such as the shared secrets store and how other MCP tools use `secretRef` values.

### `tools/`
Each MCP tool gets its own folder. The tool folder should contain its README, design notes, examples, and later source code.

### `tools/secrets/`
The first tool area. This folder is for the workings of the Secrets app itself: behavior, commands, implementation notes, examples, and eventually source code.

## Current Tool Folders

```text
tools/secrets/
tools/ai/
tools/terminal/
tools/rss/
tools/snmp/
tools/files/
tools/git/
tools/windows/
```

## Maintenance Rule

When a file is added, moved, or deleted, update this `INDEX.md` in the same change.
