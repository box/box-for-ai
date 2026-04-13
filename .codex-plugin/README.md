# Box Plugin for Codex

This directory contains Codex-specific setup notes for using this repository as
a plugin/skill source.

## Configure Box app credentials

Add your Box app OAuth credentials to
`~/.codex/config.toml`:

```toml
[mcp_servers.box]
url = "https://mcp.box.com"

[mcp_servers.box.auth]
CLIENT_ID = "<your_box_client_id>"
CLIENT_SECRET = "<your_box_client_secret>"
```

Use placeholder values in docs and never commit real credentials.

Codex uses these credentials for Box MCP authentication. The same Box app
credentials can also be used to obtain OAuth access tokens for direct REST calls.

After updating config, restart your Codex session if Box MCP tools do not appear
immediately.

## Notes

- Keep this file platform-specific. Core Box skill guidance remains in
  `skills/box/`.
- Box CLI auth remains separate (`box login`).
- For direct REST fallback, use `BOX_ACCESS_TOKEN` as an environment variable in
  your active session.
