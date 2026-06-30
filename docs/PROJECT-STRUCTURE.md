# Project Structure

The repository is organized so people can quickly find the tool they care about.

## Root

```text
README.md     # project overview
INDEX.md      # current file map
.gitignore    # local/build/runtime exclusions
```

## Documentation

```text
docs/
  PROJECT-STRUCTURE.md
```

Use `docs/` for project-wide design notes that apply to multiple tools.

## Tools

```text
tools/
  README.md
  secrets/
  ai-broker/
  rss/
  ssh/
  snmp/
  windows/
```

Each tool should follow this pattern:

```text
tools/<tool-name>/
  README.md       # what the tool does and how to use it
  DESIGN.md       # architecture, security model, decisions
  examples/       # config examples
  src/            # source code, once implementation starts
  scripts/        # setup/install helper scripts
```

## Runtime Layout

Installed binaries should not live beside runtime data.

Windows target layout:

```text
C:\Program Files\VTX\MCP\      # installed binaries
C:\ProgramData\VTX\MCP\        # shared config, work, logs
%APPDATA%\VTX\MCP\Secrets\     # per-user secrets, where applicable
```

Linux/macOS target layout will be defined later, but should follow the same split:

```text
binaries != config != secrets != work output
```

## Design Rule

MCP tools expose capabilities. The caller owns the workflow.

That caller may be Claude, another MCP client, or a VTX GUI/control panel.
