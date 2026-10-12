# EUACC — EU Funding MCP Connector

**Live European funding data inside Claude, ChatGPT, Grok and Claude Code.**

EUACC is a hosted [Model Context Protocol](https://modelcontextprotocol.io) (MCP) server that gives your AI assistant real-time, verified data on European funding — open grant calls, every EU programme, consortium partners, VCs, incubators and more — instead of stale training knowledge.

Ask *"what EU grants are open for hydrogen startups closing this quarter?"* and your AI returns real calls, real deadlines and real partners, pulled the moment you ask, straight from the official EU sources. Each answer cites its `euacc.ai` source link.

- 🌐 **Connect it:** https://www.euacc.ai/ai-tools
- 🔑 **Endpoint:** `https://www.euacc.ai/api/mcp` (sign in with EUACC via OAuth, or use a connector key)
- 💶 **Free** — 1,000 lookups/month with a free account. No card.

> EUACC is a **data connector, not a ghostwriter.** It feeds your AI the public, source-backed map of EU funding. Your project, your technology and anything you write stay inside your own AI — nothing is uploaded to EUACC.

---

## What you can ask (15 tools)

| Ask your AI… | Tool |
| --- | --- |
| "What Horizon Europe calls are open for AI in health, closing soonest?" | `search_open_calls` |
| "Is HORIZON-CL4-2026-01-DIGITAL-12 worth pursuing for us? Budget, funding rate, eligibility, official link." | `get_call` |
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

There are two ways to connect. Both use the same endpoint, `https://www.euacc.ai/api/mcp`, and the same free account.

- **Sign in with EUACC (OAuth).** The default for Claude, ChatGPT, Grok and Claude Code. You paste the endpoint, your AI opens a browser window, you sign in to EUACC and click *Allow access*. No key to copy.
- **Connector key (API key).** For clients that only accept a static header, and for headless or CI setups. You generate a key on your dashboard and send it as `Authorization: Bearer YOUR_EUACC_KEY`.

### Option A: sign in with EUACC (OAuth)

You need a free account at **[euacc.ai](https://www.euacc.ai/ai-tools)** (no card). Then:

**Claude (web & desktop):** Settings → Connectors → *Add custom connector*. Name it EUACC, paste `https://www.euacc.ai/api/mcp`, click *Add*. Claude opens EUACC in your browser; sign in and approve access.

**Claude Code**
```bash
claude mcp add --transport http euacc https://www.euacc.ai/api/mcp
```
Then run `/mcp` inside Claude Code and pick *Authenticate*; a browser window opens for the EUACC sign-in.

**ChatGPT (Plus and above, web):** turn on Developer mode, add a custom MCP connector with the endpoint, and sign in to EUACC when ChatGPT sends you there.

**Grok:** Settings → Connectors → add a custom MCP connector with the endpoint, then sign in to EUACC when prompted.

**Any other MCP client that supports OAuth** finds everything on its own from the endpoint:

| What | URL |
| --- | --- |
| Protected resource metadata (RFC 9728) | `https://www.euacc.ai/.well-known/oauth-protected-resource` |
| Authorization server metadata (RFC 8414) | `https://www.euacc.ai/.well-known/oauth-authorization-server` |
| Dynamic client registration (RFC 7591) | `https://www.euacc.ai/oauth/register` |
| Authorization (code flow, PKCE S256 required) | `https://www.euacc.ai/oauth/authorize` |
| Token | `https://www.euacc.ai/oauth/token` |
| Revocation (RFC 7009) | `https://www.euacc.ai/oauth/revoke` |

How it behaves:
- Scope is `mcp:read` (read-only data lookups). Tokens are issued only for the resource `https://www.euacc.ai/api/mcp`; a request for any other `resource` gets `invalid_target`.
- Public clients (`token_endpoint_auth_method: none`) authenticate with PKCE. Clients that register with `client_secret_post` get a `client_secret` and must send it on every token, refresh and revoke request.
- Access tokens last 30 days, refresh tokens 180 days. Each refresh returns a new refresh token and retires the old one.
- To disconnect an app, open **[your connector page](https://www.euacc.ai/dashboard/connector)** and click *Disconnect* under *Connected apps*. That revokes its tokens at once. Clients can also call the revocation endpoint themselves.

### Option B: connector key (API key)

**1. Get a key.** Sign in at **[euacc.ai](https://www.euacc.ai/dashboard/connector)** and click *Generate key*. The key is shown once, so copy it. One active key per account; revoke it on the same page to make a new one.

**2. Send it as a Bearer header.**

**Claude Code**
```bash
claude mcp add --transport http euacc https://www.euacc.ai/api/mcp \
  --header "Authorization: Bearer YOUR_EUACC_KEY"
```

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

**Clients that only speak stdio:** bridge to the remote server with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):
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

**Do I need an account?** Yes. It is free, and you use it either to sign in from your AI (OAuth) or to generate a connector key.

**Does it write my application?** No. It's a data connector; drafting happens inside your own AI, with your own private context, which EUACC never sees.

**Is my IP safe?** Yes. The connector only ever receives short search queries like "battery calls in Germany." Your project and drafts never leave your AI.

**What does it cost?** Free — 1,000 lookups/month, no card.

---

## About

Built by **[EUACC](https://www.euacc.ai)** — helping European startups get funded (EU grants + VC matching + an AI application builder).

This repository contains documentation and connection config for the hosted EUACC MCP server. The server itself is operated by EUACC.

## License

Documentation and configuration in this repository are released under the [MIT License](./LICENSE).
