# Box Agent Skills

Agent Skills to help developers using AI agents work with Box. Whether you're building Box integrations in code, working with Box content via MCP tools, configuring webhooks, or using Box AI retrieval — this plugin gives your assistant the context it needs to do it right.

The skills in this repo follow the [Agent Skills](https://agentskills.io/) format and can also be installed as a plugin for [Cursor](https://cursor.com) or [Claude Code](https://code.claude.com).

## Installation

### As an Agent Skill

```bash
npx skills add box/box-for-ai
```

Check out the latest and full list of skills [here](https://skills.sh/box/box-for-ai).

### As a Platform Plugin

This repo can also be installed as a plugin for supported platforms. You configure the Box MCP server connection through your platform's MCP settings — see the setup guide for your platform below.

| Platform | Setup guide |
|---|---|
| Cursor | [`.cursor-plugin/README.md`](.cursor-plugin/README.md) |
| Claude Code | [`.claude-plugin/README.md`](.claude-plugin/README.md) |

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected. Here are some example prompts:

### Implement Box content workflows

`Add Box file upload to my app`

`Create a shared link for this folder`

`Set up Box webhooks for new file events`

### Build document-driven flows

`Search my Box account for invoices`

`Use Box AI to classify documents`

`Wire webhooks to process new uploads`

### Troubleshoot integrations

`Debug 401 errors with my Box JWT auth`

`Fix webhook signature verification`

## Skill Structure

The Box skill follows the [Agent Skills Open Standard](https://agentskills.io/):

- `SKILL.md` - Skill manifest with frontmatter, routing table, workflow steps, and guardrails
- `references/` - Individual reference files (auth, content workflows, MCP tool patterns, AI/retrieval, etc.)
- `scripts/` - Testing and REST API helper scripts
- `examples/` - Example prompts

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
