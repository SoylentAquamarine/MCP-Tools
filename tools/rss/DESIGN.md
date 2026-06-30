# VTX-MCP-RSS Design

## Goal

Fetch and cache RSS/Atom feeds on demand.

RSS should be an MCP tool, not necessarily a background service.

## Runtime Data

```text
C:\ProgramData\VTX\MCP\RSS\feeds.json
C:\ProgramData\VTX\MCP\RSS\rss-cache.db
C:\ProgramData\VTX\MCP\Work\jobs\
```

## Workflow Example

```text
1. rss_fetch writes articles.json to a job folder.
2. ai_route summarizes articles.json.
3. files_save stores the final report.
```
