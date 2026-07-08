# EUACC — EU Funding MCP Connector

**Live European funding data inside Claude, ChatGPT, Grok and Claude Code.**

EUACC is a hosted [Model Context Protocol](https://modelcontextprotocol.io) (MCP) server that gives your AI assistant real-time, verified data on European funding — open grant calls, every EU programme, consortium partners, VCs, incubators and more — instead of stale training knowledge.

Ask *"what EU grants are open for hydrogen startups closing this quarter?"* and your AI returns real calls, real deadlines and real partners, pulled the moment you ask, straight from the official EU sources. Each answer cites its `euacc.ai` source link.

- 🌐 **Connect it:** https://www.euacc.ai/ai-tools
- 🔑 **Endpoint:** `https://www.euacc.ai/api/mcp`
- 💶 **Free** — 1,000 lookups/month with a free account. No card.

> EUACC is a **data connector, not a ghostwriter.** It feeds your AI the public, source-backed map of EU funding. Your project, your technology and anything you write stay inside your own AI — nothing is uploaded to EUACC.

---

## What you can ask (14 tools)

| Ask your AI… | Tool |
| --- | --- |
| "What Horizon Europe calls are open for AI in health, closing soonest?" | `search_open_calls` |
| "How does the EIC Accelerator work, and am I eligible?" | `get_programme` |
| "List every EU programme you cover." | `list_programmes` |
| "What EU funding is there for cleantech?" | `get_sector` |
| "What funding does Germany offer deep-tech startups?" | `get_country` |
| "How do I write an EIC Accelerator application?" | `get_guide` |
| "EIC Accelerator vs EIC Pathfinder — which fits me?" | `get_comparison` |
| "What does TRL mean?" | `get_glossary_term` |
| "Find consortium partners in Germany with a battery track record." | `search_consortium_partners` |
| "Where can I test my robotics prototype in Spain?" | `search_incubators` |
| "Which European VCs fund seed-stage climate?" | `search_vc_directory` |
| "Who's already been funded in green hydrogen?" | `search_companies` |

Plus `list_sectors` and `list_countries` for discovery. The assistant picks the right tool automatically — you just ask in plain language.

---

## Setup

### 1. Get a free key
Create a free account at **[euacc.ai](https://www.euacc.ai/ai-tools)** (no card), then generate a connector key on the dashboard. The key is shown once — copy it.

### 2. Add the connector

**Claude (web & desktop)** — Settings → Connectors → *Add custom connector*:
- URL: `https://www.euacc.ai/api/mcp`
- Authorization header: `Bearer YOUR_EUACC_KEY`

**Claude Code**
```bash
claude mcp add --transport http euacc https://www.euacc.ai/api/mcp \
  --header "Authorization: Bearer YOUR_EUACC_KEY"
```

**ChatGPT & Grok** — add a custom MCP connector under their connector settings, using the same endpoint and `Authorization` header.

**Any client via a config file** (Cursor, Windsurf, etc.):
```json
{
  "mcpServers": {
    "euacc": {
      "url": "https://www.euacc.ai/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_EUACC_KEY"
      }
    }
  }
}
```

**Clients that only speak stdio** — bridge to the remote server with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):
```json
{
  "mcpServers": {
    "euacc": {
      "command": "npx",
      "args": [
        "mcp-remote", "https://www.euacc.ai/api/mcp",
        "--header", "Authorization: Bearer YOUR_EUACC_KEY"
      ]
    }
  }
}
```

Then just ask your AI anything about EU funding.

---

## Data & coverage

| Source | Refresh | Coverage |
| --- | --- | --- |
| EU Funding & Tenders Portal | **daily** | Live open calls with real deadlines |
| CORDIS | monthly | 78,000+ consortium partners (Horizon Europe, H2020, FP7) |
| EUACC database | continuous | 15 programmes · 15 sectors · 28 countries · guides · glossary · European Digital Innovation Hubs · 1,200+ VC funds · funded-startup directory |

Every figure traces to an official EU source and is re-verified continuously.

---

## FAQ

**Which AI assistants work with it?** Claude (web, desktop, Claude Code), ChatGPT, Grok, and any MCP-compatible client.

**Do I need an account?** Yes — free, needed for the key.

**Does it write my application?** No. It's a data connector; drafting happens inside your own AI, with your own private context, which EUACC never sees.

**Is my IP safe?** Yes. The connector only ever receives short search queries like "battery calls in Germany." Your project and drafts never leave your AI.

**What does it cost?** Free — 1,000 lookups/month, no card.

---

## About

Built by **[EUACC](https://www.euacc.ai)** — helping European startups get funded (EU grants + VC matching + an AI application builder).

This repository contains documentation and connection config for the hosted EUACC MCP server. The server itself is operated by EUACC.

## License

Documentation and configuration in this repository are released under the [MIT License](./LICENSE).
