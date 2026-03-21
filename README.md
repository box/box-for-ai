# Box for AI

Your AI coding assistant already knows how to write code. This plugin teaches it Box — how to integrate the Box Platform, wire content workflows, and get the most out of every feature.

Whether you're adding Box to a new project, building shared link flows, or configuring webhooks and Box AI retrieval, just ask. The plugin gives your assistant the context it needs to do it right.

Supports **Claude Code** and **Cursor**.

## What You Can Do

**Implement Box content workflows** — Uploads, folders, folder listings, downloads and previews, shared links, collaborations, metadata, and file moves.

```
Add Box file upload to my app
Create a shared link for this folder
Set up Box webhooks for new file events
```

**Build document-driven flows** — Search, metadata, Box AI retrieval, and event-driven automations.

```
Search my Box account for invoices
Use Box AI to classify documents
Wire webhooks to process new uploads
```

**Troubleshoot integrations** — Auth issues, 401/403/404, rate limits, and wrong-actor bugs.

```
Debug 401 errors with my Box JWT auth
Fix webhook signature verification
```

## Installation

### Claude Code

```bash
/install-plugin box
```

Or from source:

```bash
/install-plugin file:///path/to/box-for-ai
```

Restart Claude Code to activate.

### Cursor

Search for **Box** in Cursor Settings > Extensions and install.

Or install from path: add the plugin path in Cursor Settings > Extensions > Install from path.

## Skills

### Content API

| Skill | Description |
|-------|-------------|
| `box-content-api` | Build and troubleshoot Box integrations for uploads, folders, downloads, shared links, collaborations, search, metadata, webhooks, and Box AI retrieval |

## Prerequisites

- **Python 3.10+** — Verification scripts use only the standard library.
- **Box CLI** (optional) — Install from [developer.box.com/guides/cli](https://developer.box.com/guides/cli) for CLI-first verification.
- **BOX_ACCESS_TOKEN** — For REST-based verification when Box CLI is unavailable.

## Quick Verification

```bash
# With Box CLI installed and authenticated:
python3 skills/box/scripts/box_cli_smoke.py check-auth
python3 skills/box/scripts/box_cli_smoke.py list-folder-items 0 --max-items 5

# With a bearer token:
export BOX_ACCESS_TOKEN="your-token"
python3 skills/box/scripts/box_rest.py get-item --item-type folder --item-id 0
```

## Contributing

Skills follow the [Agent Skills specification](https://agentskills.io). The Box skill is a directory with a `SKILL.md` file containing YAML frontmatter and markdown instructions, plus a `references/` directory for feature-specific deep dives.

## License

MIT
