# Composio — App Integration Platform

**Type:** SOP
**Category:** Operations
**Source:** dashboard.composio.dev
**Date:** 2026-06-23
**Related:** [[SYSTEM.md]]

---

## What it is

Composio connects 1,000+ apps (Google Calendar, Notion, GitHub, Slack, etc.) to AI agents via a single MCP server. Instead of building separate integrations, agents call Composio tools which handle OAuth, auth, and API calls for each connected app.

## Installation

Installed via MCP on 2026-06-23.

- **Workspace:** quincy_workspace
- **MCP URL:** https://connect.composio.dev/mcp
- **Auth:** X-CONSUMER-API-KEY (stored in Jermes.AI / Claude Code MCP config)

Available install targets: Claude Cowork, Claude Code, Codex, ChatGPT, Cursor, VS Code, Notion.

## How it works

1. Composio exposes tools dynamically — only apps you've **connected (OAuth authorized)** appear as callable tools on the MCP server.
2. Use `COMPOSIO_SEARCH_TOOLS` to discover the exact slug for any action (e.g. `GOOGLECALENDAR_EVENTS_LIST`).
3. Call the slug with required params — Composio handles the API request.

## Important: Tool not found errors

If you get MCP error -32602 ("Tool not found") even after slug discovery confirms the name, the app is not connected. Fix:
1. Go to dashboard.composio.dev → Connected Accounts
2. Connect and authorize the app for `quincy_workspace`
3. Retry the tool call

## Apps to connect

- [x] Google Calendar — connected 2026-06-23
- [ ] Gmail
- [ ] Notion
- [ ] Slack
- [ ] GitHub
