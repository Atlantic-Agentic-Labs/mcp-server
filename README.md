<img src="logo.png" alt="Atlantic Agentic Labs" width="96" height="96">

# Atlantic Agentic Labs MCP server

A public, read-only MCP server for searching and reading the LightHouse™
documentation and the items Atlantic Agentic Labs has published.

| | |
|---|---|
| Endpoint | `https://ask.atlanticagenticlabs.com/mcp` |
| Transport | Streamable HTTP (remote; nothing to install) |
| Authentication | None |
| Registry name | `com.atlanticagenticlabs/ask` |
| Documentation | https://docs.atlanticagenticlabs.com/mcp/public/ |

The server is operated by Atlantic Agentic Labs. This repository describes
it for MCP registries and clients. It does not contain the server's source
code.

## Tools

| Tool | What it returns |
|---|---|
| `search_docs` | Exact quoted passages from the public LightHouse documentation, the LightHouse OpenAPI document and the LightHouse MCP reference, each with a deep link and the document version. |
| `search_published_knowledge` | Exact quoted passages from items Atlantic Agentic Labs has published, each with a deep link and the publication date. |
| `read_page` | One public documentation page as Markdown, by URL or path, with its title, URL and last-updated date. Long pages are truncated with a note and the page URL. |
| `list_pages` | The public documentation's table of contents: section, title, path and a one-line description for each page. |

All four tools are read-only (`readOnlyHint: true`) and closed-world
(`openWorldHint: false`). The server makes no model calls: it returns
quotations from the source rather than generated text, so every answer can
be checked against the original. It holds no tenant or customer data and
cannot change anything on the caller's behalf.

## Configuration

### VS Code and GitHub Copilot

Add the server to `.vscode/mcp.json` in a workspace, or to your user
settings:

```json
{
  "servers": {
    "atlantic": {
      "type": "http",
      "url": "https://ask.atlanticagenticlabs.com/mcp"
    }
  }
}
```

The tools are then available to Copilot Chat in agent mode.

### Claude Code

```sh
claude mcp add --transport http atlantic https://ask.atlanticagenticlabs.com/mcp
```

### Cursor

Add to `~/.cursor/mcp.json` or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "atlantic": {
      "url": "https://ask.atlanticagenticlabs.com/mcp"
    }
  }
}
```

### Other clients

Any MCP client that supports Streamable HTTP can connect to
`https://ask.atlanticagenticlabs.com/mcp` with no headers or credentials.
Instructions for further clients are in the
[documentation](https://docs.atlanticagenticlabs.com/mcp/public/).

## Usage

Ask the assistant a question about LightHouse or Atlantic, for example:

- "How does LightHouse record approvals? Quote the documentation."
- "Which LightHouse API endpoint returns usage by pack?"
- "List the documentation pages about connectors."

The assistant calls `search_docs` or `search_published_knowledge`, then
`read_page` when it needs a whole page, and cites the links returned.

Requests are rate-limited per caller and overall. On HTTP 429, wait for the
number of seconds in the `Retry-After` header before trying again.

## Privacy

Questions sent to the server are recorded in redacted form, without
identifiers, and kept for 12 months to improve search quality. See the
[privacy policy](https://www.atlanticagenticlabs.com/legal/privacy).

## Support

solutions@atlanticagenticlabs.com

Security reports: see
[security.txt](https://atlanticagenticlabs.com/.well-known/security.txt).

© 2026 Atlantic Agentic Labs. Licensed under Apache-2.0.
