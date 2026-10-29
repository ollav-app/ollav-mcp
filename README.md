# Ollav MCP

Ollav is where humans and AI agents review HTML together. Your agent shares a prototype as a live link, your team pins comments to the exact element they mean, and the agent reads those comments, fixes the page and publishes the next version at the same URL.

This repo holds Ollav's Claude Code plugin and MCP setup. The server itself is hosted: there is nothing to run.

- Endpoint (Streamable HTTP): `https://app.ollav.app/mcp`
- API keys: https://app.ollav.app/keys (`ol_test_...` for a sandbox, `ol_live_...` for real work)
- Full docs: https://ollav.app/docs/mcp · Quickstart: https://ollav.app/docs/agents
- MCP Registry: `app.ollav/ollav`

## Claude Code

Install the plugin (the MCP server plus a skill that tells Claude when to use Ollav):

```
/plugin marketplace add ollav-app/ollav-mcp
/plugin install ollav@ollav
```

Set `OLLAV_API_KEY` in your environment first. Or add just the MCP server:

```bash
claude mcp add --transport http ollav https://app.ollav.app/mcp --header "Authorization: Bearer $OLLAV_API_KEY"
```

Check it: `claude mcp list` shows `ollav` as connected.

## Claude.ai and Claude Desktop

Settings → Connectors → Add custom connector. Name: `Ollav`, URL: `https://app.ollav.app/mcp`. Claude signs you in with OAuth and you pick the workspace it may use.

## Cursor

`~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "ollav": {
      "url": "https://app.ollav.app/mcp",
      "headers": { "Authorization": "Bearer ${env:OLLAV_API_KEY}" }
    }
  }
}
```

## ChatGPT

Settings → Apps & Connectors → Create (developer mode). Name: `Ollav`, MCP server URL: `https://app.ollav.app/mcp`, Authentication: OAuth. Attach an HTML file and say "share this on Ollav".

## OpenAI Codex CLI

```bash
codex mcp add ollav --url https://app.ollav.app/mcp --bearer-token-env-var OLLAV_API_KEY
```

Or in `~/.codex/config.toml`:

```toml
[mcp_servers.ollav]
url = "https://app.ollav.app/mcp"
bearer_token_env_var = "OLLAV_API_KEY"
```

## Any other MCP client

Point it at `https://app.ollav.app/mcp` and send `Authorization: Bearer <key>`. Clients that only run local stdio servers can bridge with `npx -y mcp-remote https://app.ollav.app/mcp --header "Authorization: Bearer $OLLAV_API_KEY"`.

## Tools

| Tool | What it does |
| --- | --- |
| `share_html` | Publish an HTML file and get a link people can comment on |
| `publish_new_version` | Update something already shared: same URL, comments kept |
| `list_documents` | Find documents shared earlier |
| `get_document` | Metadata, share URL and version history |
| `get_review` | Open feedback threads as Markdown + JSON, with a polling cursor |
| `get_comments` | Every comment, resolved ones too |
| `add_comment` | Pin a comment to an element, or reply |
| `resolve_comment` | Mark a thread done (or reopen it) |
| `react_to_comment` | Acknowledge a comment with an emoji |
| `delete_comment` | Delete a comment and its replies |
| `export_data` | Signed link to a ZIP of your data |

## The loop

`share_html` → people comment → `get_review` → edit → `publish_new_version` → `resolve_comment`.

## License

MIT. This repo holds configuration only; use of the hosted service is covered by Ollav's terms at https://ollav.app.
