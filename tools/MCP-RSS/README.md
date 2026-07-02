# MCP-RSS

`MCP-RSS` is the planned RSS feed fetch/cache tool.

It is a safe early test tool because most feeds do not need secrets, but authenticated feeds can use `secretRef` later.

## Planned MCP Tools

```text
rss_add_feed
rss_list_feeds
rss_fetch
rss_check_updates
rss_get_unread
rss_mark_read
rss_export_items
```

## Uses Secrets Store

Optional authenticated feeds may use:

```text
rss/feed-name/basic-password
rss/feed-name/token
```

## Status

Skeleton only.
