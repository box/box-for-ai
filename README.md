# Box Agent Skills

Agent Skills to help developers using AI agents work with Box. Whether you're adding Box to a new project, building shared link flows, configuring webhooks or using Box AI retrieval, this plugin gives your assistant the context it needs to do it right.

The skills in this repo follow the [Agent Skills](https://agentskills.io/) format.

## Installation

```bash
npx skills add box/box-for-ai
```

Check out the latest and full list of skills [here](https://skills.sh/box/box-for-ai).

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected. Here are some example prompts for different use cases:

 

|    **Implement Box content workflows**    | 
| ------------------------------------------| 
| Add Box file upload to my app             | 
| Create a shared link for this folder      | 
| Set up Box webhooks for new file events   | 

 — Search, metadata, Box AI retrieval, and event-driven automations.

|    **Build document-driven flows**        | 
| ------------------------------------------| 
| Search my Box account for invoices        | 
| Use Box AI to classify documents          | 
| Wire webhooks to process new uploads      | 


 — Auth issues, 401/403/404, rate limits, and wrong-actor bugs.

|    **Troubleshoot integrations**          | 
| ------------------------------------------| 
| Debug 401 errors with my Box JWT auth     | 
| Fix webhook signature verification        | 

## Skill Structure

The Box skill follows the [Agent Skills Open Standard](https://agentskills.io/):

- `SKILL.md` - Required skill manifest with frontmatter (name, description, metadata)
- `references/` - Individual reference files
- `scripts/` - Testing a REST API helper scripts
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
