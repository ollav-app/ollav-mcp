---
name: ollav
description: Use when the user asks to share, preview, host, publish, send round, get feedback on or review an HTML file, prototype, mockup or generated page, or to act on review comments left on something shared with Ollav.
---

# Ollav

Ollav hosts one self-contained HTML file at a live link where people pin comments to elements. The `ollav` MCP server is bundled with this plugin.

## Share
- File on disk and a shell available: upload with curl so it costs no tokens:
  `curl -X POST https://app.ollav.app/api/documents -H "Authorization: Bearer $OLLAV_API_KEY" -H "X-Ollav-Client: agent" -F "file=@dist/index.html"`
- Otherwise call `share_html` with the HTML. Always give the user the returned `url`; keep the `slug`.
- Building a React or Vue app? Bundle to one file (e.g. vite-plugin-singlefile) first. Relative asset paths break; inline them or use absolute URLs. Limit 15 MB.
- Leave visibility at `workspace` unless the user explicitly asks for `link` or `public`.

## Review loop
1. `get_review` with the slug: open threads as Markdown + JSON, plus a `cursor`.
2. Edit the source.
3. `publish_new_version` with the same slug (same URL, comments kept). Never `share_html` again for an update.
4. `resolve_comment` on the threads you fixed; optionally `add_comment` to say what changed.
5. To check for new feedback later, pass the previous `cursor` as `since`. Don't poll in a tight loop.

Docs: https://ollav.app/docs/mcp
