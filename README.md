# devBoard MCP server

[![smithery badge](https://smithery.ai/badge/darko-bacic/devboard)](https://smithery.ai/servers/darko-bacic/devboard)

devBoard is a simple project tracker for freelancers and small dev teams: boards, epics, releases and a timer, in an app for iPhone, Android and Mac. This is its MCP server. Connect it once and your AI assistant can read the task it is working on, open new ones, comment, move cards and track your time. It acts as you, in one workspace, with your role.

- Server: `https://api.getdevboard.app/mcp` (Streamable HTTP)
- Auth: `Authorization: Bearer <token>`; make the token in the Mac app under **Settings → Developer**
- Setup page with every client: https://getdevboard.app/mcp
- Website: https://getdevboard.app

## Quick start

Claude Code:

```bash
claude mcp add --transport http --scope user devboard https://api.getdevboard.app/mcp \
  --header "Authorization: Bearer <your-token>"
```

Cursor (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "devboard": {
      "url": "https://api.getdevboard.app/mcp",
      "headers": { "Authorization": "Bearer <your-token>" }
    }
  }
}
```

VS Code (`.vscode/mcp.json`, asks for the token once and keeps it out of the file):

```json
{
  "inputs": [
    { "type": "promptString", "id": "devboard-token", "description": "devBoard token", "password": true }
  ],
  "servers": {
    "devboard": {
      "type": "http",
      "url": "https://api.getdevboard.app/mcp",
      "headers": { "Authorization": "Bearer ${input:devboard-token}" }
    }
  }
}
```

Claude Desktop goes through `mcp-remote`; the exact block is on https://getdevboard.app/mcp.

Then ask: "What's on my plate today?" or "Start APP-42."

## Tools (29)

| Area | Tools |
|---|---|
| Find and read | `get_workspace`, `search_items`, `get_item`, `my_work`, `run_view` |
| Tickets | `create_item`, `update_item`, `move_item`, `archive_item` |
| Comments | `add_comment`, `edit_comment`, `delete_comment` |
| Dev loop | `start_work` (assigns you, moves the card to In progress, starts the timer, returns a branch name), `finish_work` (stops the timer, comments, moves the card) |
| Time | `start_timer`, `stop_timer`, `log_time`, `timesheet`, `edit_time_entry`, `delete_time_entry` |
| Personal to-dos | `list_todos`, `add_todo`, `update_todo`, `delete_todo` |
| Inbox | `list_notifications`, `mark_notifications_read` |
| Admin only | `create_release`, `ship_release`, `create_label` |

It cannot delete items, change workspace settings, or manage members and invites. Those stay in the app.

## Safety

- A token acts as you, with your role. A member's token can do what a member can do and nothing more.
- A token belongs to one workspace, the one that was open when you made it. Several clients means several tokens.
- Revoke a token in **Settings → Developer** and it stops working at once. devBoard stores only a hash of it.
- Guests can read a workspace but cannot create tokens.

More: https://getdevboard.app/guides/mcp-token-security

## REST without MCP

The same token works on a small REST API at `https://api.getdevboard.app/dev/v1`: `GET me`, `GET projects`, `GET items?project=&q=`, `GET items/{KEY}`, `GET items/{KEY}/pulls`, `POST items/{KEY}/status`. Guide: https://getdevboard.app/guides/rest-api-from-ci

## Pull requests from GitHub

A pull request that mentions an item key (APP-42) in its title, body or branch shows on that item and can move the card when it opens, merges or closes. One webhook per repository: https://getdevboard.app/guides/link-github-pull-requests

## As a plugin

This repository is also an [Agent Plugins](https://agent-plugins.org) package (`plugin.json` + `mcp.json`), so plugin directories such as cursor.directory can import it. The plugin file lists the server URL only. Plugins must not carry secrets, so after installing it add your own token as an `Authorization: Bearer <token>` header in your client, as shown above.

## Where it is listed

- [Official MCP Registry](https://registry.modelcontextprotocol.io/?search=app.getdevboard) as `app.getdevboard/devboard`
- [Smithery](https://smithery.ai/servers/darko-bacic/devboard)

## About this repository

This repository holds the registry listing (`server.json`) and documentation for the hosted server. The server itself is part of the devBoard backend and is not open source. Questions: hello@getdevboard.app
