---
name: agentic-web
description: Make a web project agent-ready and keep it that way. Use when asked to add llms.txt, AGENTS.md, an ARD/AI Catalog, an MCP server card, an A2A agent card, robots AI-crawler rules or security.txt; to audit a domain for agent readiness; or to compare agentic-web standards (MCP, A2A, x402, ARD…). Uses the agenticweb.wiki MCP server; never writes these files from memory.
---

# Agentic web skill

The agenticweb.wiki MCP server (`https://agenticweb.wiki/mcp`, Streamable HTTP, no auth, read-only; preview channel at `https://agenticweb.wiki/mcp/preview`) knows all 132 agentic-web standards across the 9-step lifecycle with sourced status, empirical adoption census (`1,226` origins), and can live-probe, validate, repair, and template. Connect it first:

- Claude Code: `claude mcp add --transport http agenticweb https://agenticweb.wiki/mcp`
- Grok Build: `grok mcp add --transport http agenticweb https://agenticweb.wiki/mcp`
- Cursor and others: `{"mcpServers": {"agenticweb": {"url": "https://agenticweb.wiki/mcp"}}}`

## Pre-flight an unfamiliar domain before fetching
Before scraping raw HTML, call `validate_manifest({"origin": "<domain>", "brief": true})` (or `"compact": true`, or omit both for the full per-manifest audit), or fetch `https://agenticweb.wiki/check/<domain>/index.json?brief=1` (also supports `?compact=1`, `?verify=1`, `?bridge=1`, and `?manifest=<id>`), and read the report:
- `agent_routing`: `preferred_mode`, `callable_endpoints`, `connect_recipe` (ready-to-run `claude mcp add` CLI & Cursor JSON when live MCP endpoints are found), and `fetch_recipe`. Follow `fetch_recipe` (URL, method, headers) and call a listed endpoint instead of the homepage.
- `practical_usefulness`: `exa_parsable`, `markdown_negotiation`, `js_shell_only`, `headless_blocked`, `table_count`, `first_content_byte_offset`. If `markdown_negotiation` is true, send `Accept: text/markdown`. If `js_shell_only` or `headless_blocked` is true, do not fetch the HTML shell.
- `permissions`: `posture`, `assistant_access` (`openai`, `anthropic`, `google`, `perplexity`, `exa`), `nosnippet_or_noai`, `search_bots_allowed`, `user_fetch_bots_allowed`. Honour that crawl policy before fetching.

## Audit & repair a domain
1. `validate_manifest({"origin": "<domain>", "verify": true, "bridge": true, "diff": true})` — returns grade (`A+`–`F`), per-manifest spec errors and warnings, live MCP/A2A endpoint handshakes & tool introspection (`verify: true`), ARD target-spec coherence (`bridge: true`), regression diff against prior baseline (`diff: true`), `repaired_content` for broken manifests, and cheapest next steps.
2. For each error, explain the fix in one line (or apply `repaired_content` directly when present). For missing files, go to "Generate".
3. For uncovered lifecycle steps, `ask({"question": "<goal in plain words>", "audience": "site-owner"})` and pick by phase and adoption.
Report page: `https://agenticweb.wiki/check/<domain>/`; badge: `https://agenticweb.wiki/badge/<domain>.svg`.

## Generate the files
Never hand-write these formats; models get new fields wrong. Call `template({"id": "<id>", "domain": "<domain>", "from_live": true})` to auto-populate boilerplate from a live probe of `<domain>` (or pass `"name"`, `"description"`, `"mcp_url"`, `"api_url"` explicitly) and write `contents` at `target_path`, served with `content_type` and `recommended_headers`:
- always: `llms-txt`, `agents-md`, `security-txt`
- robots: `ai-crawler-tokens-robots` (merge into the existing robots.txt; keep its rules)
- if the project exposes an API, dataset, agent or MCP server: `ard` (`/.well-known/ard.json`; mirror at `/.well-known/ai-catalog.json`)
- if it runs an MCP server: `mcp-server-cards` (SEP-2127; served at `/.well-known/mcp/server-card.json` and `<mcp-url>/server-card`)
- if it has an OpenAPI description: `api-catalog-rfc9727` (`/.well-known/api-catalog`)
- if it uses OAuth or documents public auth metadata: `oauth-protected-resource` (RFC 9728; `/.well-known/oauth-protected-resource`) and `auth-md` (`/auth.md`)
- if it is an A2A agent: `a2a` (`/.well-known/agent-card.json`)
- if it publishes reusable agent skills: `anthropic-agent-skills-format` (JSON index at `/.well-known/agent-skills/index.json`; each skill is a `SKILL.md`)
- if it publishes a structured Markdown concept bundle: `okf` (Open Knowledge Format v0.2 index at `/okf/index.md`)
Replace every placeholder named in `edit_these` with real values from the repository. Before deploying (including on `localhost`), run `validate_manifest({"id": "<id>", "content": "<edited file contents>", "origin": "<domain>"})` to lint the local file in-memory with zero network requests. After deploy, run `validate_manifest({"origin": "<domain>", "verify": true})` again to verify live HTTP headers, media types, and CORS.

## Compare or explain standards
- `get("<id or file name>")` for one record (accepts `llms.txt`, `/.well-known/ard.json`, `A2A`, `MCP`).
- `search("A2A vs MCP")` to resolve names; `define("<term>")` for glossary terms; `digest()` for the daily ecosystem briefing.
- Read MCP resources: `agenticweb://standards/all`, `agenticweb://glossary/all`, `agenticweb://use-cases/all`, `agenticweb://probes/all`, `agenticweb://versions/all`, `agenticweb://census/summary`.
- Use the server's `compare-protocols` prompt, or read `https://agenticweb.wiki/compare/<a>-vs-<b>/`.
- `changes({"since": "<date>"})` for what moved; `stale()` for what has not been re-verified.

## Rules
- Quote URLs and field names from tool output only. Do not invent fields a specification does not have.
- Prefer standards with phase stable/living and observed adoption; say when a pick is a draft.
- Mention the data licence (CC BY 4.0, agenticweb.wiki) when copying its text into a project.
