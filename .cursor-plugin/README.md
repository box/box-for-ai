# Box Plugin for Cursor

Search, read, and manage your Box content directly from Cursor — plus tap into
Box AI for Q&A, summarization, and metadata extraction without leaving your
editor.

When installed as a Cursor plugin, the Box MCP server is configured
automatically — giving the agent direct access to your files, folders, hubs,
and Box AI tools.

## Prerequisites

You'll need a Box account with access to the
[Box MCP server](https://developer.box.com/guides/box-mcp/remote/). Your Box
admin may need to enable the MCP server integration in the
[Admin Console](https://developer.box.com/guides/authorization/).

### Create a Box OAuth 2.0 app

Head to the [Box Developer Console](https://app.box.com/developers/console)
and [create a new Custom App with OAuth 2.0 authentication](https://developer.box.com/guides/authentication/oauth2/oauth2-setup/).
Grab your **Client ID** and **Client Secret** from the app's Configuration
tab, then add the following redirect URI:

```
cursor://anysphere.cursor-mcp/oauth/callback
```

This allows Cursor to complete the OAuth authentication flow with Box.

### Set your environment variables

The plugin's `mcp.json` registers the Box MCP server and references two
environment variables for credentials:

```sh
export BOX_CLIENT_ID="your_client_id"
export BOX_CLIENT_SECRET="your_client_secret"
```

Add these to your shell profile (`.zshrc`, `.bashrc`, etc.) or use whatever
secrets management your team prefers.

> **Important:** Cursor must be launched from a shell that has these env vars.
> If you open Cursor from Spotlight or the macOS dock, it will not inherit
> them. After adding the exports, open a **new** terminal and launch Cursor
> from there (`cursor .` or `open -a Cursor`).

> **Why not `~/.cursor/mcp.json`?** The plugin includes a repo-local
> `mcp.json` that registers the Box MCP server. Cursor prioritizes repo-local
> MCP config over the global `~/.cursor/mcp.json` for the same server name,
> so credentials placed in the global file will not be used while this plugin
> is active. Environment variables are the recommended approach.

## Installation

**From the Marketplace (recommended):**
Install the Box plugin from the [Cursor Marketplace](https://cursor.com/marketplace).

**Manual installation:**

1. Clone this repository and copy it into `~/.cursor/plugins/`:
   ```sh
   git clone https://github.com/box/box-for-ai.git
   cp -R box-for-ai ~/.cursor/plugins/box
   ```
2. Register the plugin in `~/.claude/plugins/installed_plugins.json` (create
   the file if it doesn't exist):
   ```json
   {
     "plugins": {
       "box@local": [
         {
           "scope": "user",
           "installPath": "/Users/<you>/.cursor/plugins/box"
         }
       ]
     }
   }
   ```
3. Enable it in `~/.claude/settings.json`:
   ```json
   {
     "enabledPlugins": {
       "box@local": true
     }
   }
   ```
4. In Cursor Settings, make sure **Include third-party Plugins, Skills, and
   other configs** is enabled under **Features**.
5. Restart Cursor.

The MCP server connection, skill, and rules are all configured automatically
once the plugin is installed.

## What's included

| File | What it does |
|---|---|
| `mcp.json` | Registers the remote Box MCP server at `https://mcp.box.com`. Do not edit — credentials are supplied via environment variables, not this file. |
| `skills/box/SKILL.md` | Teaches the agent Box best practices — API integration patterns, MCP tool usage, and workflow guidance |
| `rules/box.mdc` | Enforces safety guardrails — confirmation prompts for destructive actions, hub modifications, and content display preferences |

## Plugin metadata

- `plugin.json` - Plugin manifest (name, description, keywords, logo, homepage)
- `marketplace.json` - Marketplace listing for plugin discovery

## Links

- [Box MCP Server docs](https://developer.box.com/guides/box-mcp/remote/)
- [Box OAuth 2.0 setup](https://developer.box.com/guides/authentication/oauth2/oauth2-setup/)
- [Box Developer Console](https://app.box.com/developers/console)
- [Cursor Plugin Docs](https://cursor.com/docs/plugins)
