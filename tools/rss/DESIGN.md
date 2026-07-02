# VTX-MCP-RSS Design

## Goal

Fetch and cache RSS/Atom feeds on demand.

RSS should be an MCP tool, not necessarily a background service.

## Runtime Data

```text
%ProgramData%\VTX-MCP\MCP-RSS\feeds.json
%ProgramData%\VTX-MCP\MCP-RSS\rss-cache.db
%ProgramData%\VTX-MCP\MCP-Console\jobs\
```

## Workflow Example

```text
1. rss_fetch writes articles.json to a job folder.
2. ai_route summarizes articles.json.
3. files_save stores the final report.
```
