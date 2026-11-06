# Promvia MCP server

[![mcptry](https://mcptry.com/badge/app.promvia/promvia)](https://mcptry.com/servers/app.promvia/promvia)

Promvia's official hosted [Model Context Protocol](https://modelcontextprotocol.io/) server. Promvia measures where ChatGPT, Perplexity, Gemini, Google AI Overviews and other AI engines mention and cite a website, and which sources they cite instead. Through this server an assistant reads those results and acts on them.

- **Endpoint:** `https://promvia.app/api/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 with PKCE. Clients discover it from the 401 (protected-resource metadata) and register themselves, so there is no API key, client ID or secret to configure. On the consent screen you choose which of your sites the connection may use and whether it may write.
- **MCP Registry:** `app.promvia/promvia` ([server.json](server.json))
- **Docs:** https://promvia.app/developers

The hosted server's implementation is private. This repository holds its registry manifest, the client setup and the tool reference.

## Connect

**Claude Code**

```bash
claude mcp add --transport http promvia https://promvia.app/api/mcp
```

**Cursor**: install the Promvia plugin from the marketplace, or add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "promvia": { "url": "https://promvia.app/api/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "promvia": { "type": "http", "url": "https://promvia.app/api/mcp" }
  }
}
```

**Claude and ChatGPT**: add Promvia from the connector or app directory, or as a custom connector with the endpoint above.

## Tools

| Category | Tools |
| --- | --- |
| Sites | `list_sites`, `select_site`, `current_site` |
| Visibility | `get_visibility_score`, `get_citations`, `get_citation_history`, `get_evidence`, `get_sources_report`, `get_metrics` |
| Fixes | `get_geo_audit`, `get_geo_actions`, `get_opportunities`, `get_deep_scan` |
| Prompts | `list_prompts`, `get_prompt_templates` |
| Account | `get_usage`, `get_capabilities` |
| Write (own scope) | `add_prompt`, `archive_prompt`, `set_prompt_answer`, `sync_prompt_answers`, `start_citation_run`, `start_deep_scan`, `generate_brief`, `update_action_status` |

25 tools: 17 read-only and 8 write tools. Every tool declares `readOnlyHint` / `destructiveHint`, and write tools need the write scope granted at consent. The server's `tools/list` is the source of truth for names and schemas.

Start with `list_sites`, then `select_site`. A connection works on one selected site at a time; every site tool answers for that site only, and a site you did not grant is refused.

## Plans

A free Promvia plan exists. MCP read tools need a Starter plan or above, write tools Pro or above. Tool calls count against the account's API allowance, which `get_usage` reports.

## Support

info@atlaven.com

## License

The files in this repository are MIT licensed. The Promvia name and logo are trademarks of Atlaven and are not covered by the MIT license. The Promvia service is governed by its terms at https://promvia.app/terms.
