# Box MCP Tool Patterns

Best-practice patterns for working with Box content via the Box MCP server. The MCP schema already describes each tool's parameters — this reference focuses on **how** to use them well.

> **Mandatory guardrails live in `rules/box.mdc` (at the repo root).** That file covers confirmation gates for destructive actions, hub modifications, file comments, Doc Gen output locations, externally shared folders, content display preferences, and Box AI governance. Read and follow them in every session.

## MCP auth and setup

The Box MCP server authenticates via OAuth 2.0. The MCP server connection is configured by the user through their platform's MCP settings (e.g., `~/.cursor/mcp.json` in Cursor). This keeps credentials in the user's own config and avoids the complexity of environment variable resolution.

The Box OAuth app must also have the platform's redirect URI registered (e.g., `cursor://anysphere.cursor-mcp/oauth/callback` for Cursor). See `references/auth-and-setup.md` for detailed step-by-step instructions on creating an OAuth app, retrieving credentials, and configuring the MCP server.

If MCP tools are not appearing in the session:

1. Check `~/.cursor/mcp.json` for a Box server entry. If it contains a `box` server with `CLIENT_ID` and `CLIENT_SECRET` values, the MCP server should be available. Verify by calling `who_am_i`. If it fails, the OAuth flow may not have been completed — call `mcp_auth` to trigger it.
2. If no Box server entry exists, guide the user through `references/auth-and-setup.md` to create or retrieve OAuth credentials and add the server:

```json
{
  "mcpServers": {
    "box": {
      "url": "https://mcp.box.com",
      "auth": {
        "CLIENT_ID": "<client_id>",
        "CLIENT_SECRET": "<client_secret>"
      }
    }
  }
}
```

If the file already contains other MCP servers, merge the `box` entry into the existing `mcpServers` object — do not overwrite the file. Never write credentials into the conversation or into files inside a repository.

3. Confirm the OAuth app has the correct redirect URI for the platform.
4. Confirm the platform has third-party plugins enabled (e.g., in Cursor: Settings > Features > "Include third-party Plugins, Skills, and other configs").
5. Restart the editor only as a last resort — MCP connections are established at startup.

If MCP auth still fails after setup, fall back to the Box CLI while the user resolves the connection. See `references/box-cli.md` for CLI auth and common commands.

## When to use MCP vs CLI

Tool selection between MCP and CLI is handled in the main skill workflow — see the tool selection table in `SKILL.md`. In short: MCP provides structured I/O, plugin-managed auth, and concurrent-safe calls; CLI provides full API coverage and compact, field-filtered output. The workflow picks the best available tool per operation.

## Search

- When the user refers to a folder by name, use `search_folders_by_name` first to resolve the folder ID, then scope subsequent file searches to that folder.
- Prefer `search_files_metadata` over keyword search when the user's query maps to known metadata fields or templates — it returns more precise results.

## File writes

- When uploading new files with `upload_file`, prefer writing to a dedicated output folder rather than the root or the same folder as source files. Ask the user where to place output if it isn't obvious.

## Metadata and structured extraction

- Prefer `ai_extract_structured` over `ai_extract_freeform` when a metadata template exists. Check for available templates first (e.g., via context or prior results) and infer the best match. Only ask the user if no template can be determined. Always tell the user which template was selected.
- When extracting from multiple files, process them in batches and summarize results clearly — don't dump raw JSON unless asked.
- Use `ai_extract_structured_from_fields_enhanced` when the user needs custom field definitions without a pre-existing template.

## Box AI

- Use `get_file_content` to retrieve file content.
- Use `ai_qa_single_file` for questions about one document, `ai_qa_multi_file` for cross-file analysis, and `ai_qa_hub` when the user has an existing hub.
- Box AI responses include citations — surface them when possible so the user can verify answers.

## General guidelines

- Call `who_am_i` at the start of a session if you need to verify permissions or identify the authenticated user.
- When listing folder contents, paginate if needed and summarize large directories rather than printing every item.
- Prefer reading file details with `get_file_details` before operating on a file — it confirms the file exists and shows current state.
- For exploratory or demo usage, prefer working within a dedicated folder rather than operating across the user's entire Box account.
- Avoid granting or assuming broad enterprise-wide access. Default to least-privilege — only access the folders and files the task requires.
