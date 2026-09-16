# mcp-pacer

PACER Case Locator (PCL) MCP — live US federal court case & party search on the

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1576+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `pacer_case_search` | Find US federal court cases by party name, case title, or case number — live search of the PACER Case Locator (PCL) across every federal district, bankruptcy, and appellate court. Returns case number, title, court, filing date, nature of suit, and case type. Requires your own PACER credentials (NextGen); PACER charges your account per search ($0.10/page, capped). Example: pacer_case_search({ case_title: "Lytx v. Sanderson", _apiKey: "myuser:mypass" }) or pacer_case_search({ party_name: "Nicholas Henderson", court_id: "ilndc", _apiKey: "myuser:mypass" }) |
| `pacer_party_search` | Find parties across US federal court cases by name — live PACER Case Locator (PCL) party search across all federal courts. Returns each matching party (name, role) with the associated case number, court, and case title. Requires your own PACER credentials (NextGen); PACER charges your account per search ($0.10/page, capped). Example: pacer_party_search({ party_name: "Nicholas Henderson", _apiKey: "myuser:mypass" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "pacer": {
      "url": "https://gateway.pipeworx.io/pacer/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/pacer/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1576+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "pacer": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-pacer"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-pacer
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Pacer data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
