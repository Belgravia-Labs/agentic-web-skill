---
name: agentic-web
description: Make a web project agent-ready and keep it that way. Use when asked to add llms.txt, AGENTS.md, an ARD/AI Catalog, an MCP server card, an A2A agent card, robots AI-crawler rules or security.txt; to audit a domain for agent readiness; or to compare agentic-web standards (MCP, A2A, x402, ARD…). Uses the agenticweb.wiki MCP server; never writes these files from memory.
---

# Agentic web skill

The agenticweb.wiki MCP server (`https://agenticweb.wiki/mcp`, Streamable HTTP, no auth, read-only) knows the agentic-web standards on the map (the live count is on the homepage) with sourced status, and can probe, validate and template. Connect it first:

- Claude Code: `claude mcp add --transport http agenticweb https://agenticweb.wiki/mcp`
- Grok Build: `grok mcp add --transport http agenticweb https://agenticweb.wiki/mcp`
- Cursor and others: `{"mcpServers": {"agenticweb": {"url": "https://agenticweb.wiki/mcp"}}}`

## Pre-flight an unfamiliar domain before fetching
Before scraping raw HTML, call `validate_manifest({"origin": "<domain>"})`, or fetch `https://agenticweb.wiki/check/<domain>/index.json`, and read the report:
- `agent_routing`: `preferred_mode`, `callable_endpoints`, and `fetch_recipe`. Follow `fetch_recipe` (URL, method, headers) and call a listed endpoint instead of the homepage.
- `practical_usefulness`: `exa_parsable`, `markdown_negotiation`, `js_shell_only`, `headless_blocked`, `table_count`, `first_content_byte_offset`. If `markdown_negotiation` is true, send `Accept: text/markdown`. If `js_shell_only` or `headless_blocked` is true, do not fetch the HTML shell.
- `permissions`: `posture`, `assistant_access` (`openai`, `anthropic`, `google`, `perplexity`, `exa`), `nosnippet_or_noai`, `search_bots_allowed`, `user_fetch_bots_allowed`. Honour that crawl policy before fetching.

## Audit a domain
1. `validate_manifest({"origin": "<domain>"})` — grade, per-manifest errors (spec violations) and warnings, steps covered, what an agent can call, cheapest next steps.
2. For each error, explain the fix in one line. For missing files, go to "Generate".
3. For uncovered lifecycle steps, `ask({"question": "<goal in plain words>"})` and pick by phase and adoption.
Report page: `https://agenticweb.wiki/check/<domain>/`; badge: `https://agenticweb.wiki/badge/<domain>.svg`.

## Generate the files
Never hand-write these formats; models get new fields wrong. For each file call `template({"id": "<id>", "domain": "<domain>", "name": "...", "description": "...", "mcp_url": "...", "api_url": "..."})` and write `contents` at `target_path`, served with `content_type` and `recommended_headers`:
- always: `llms-txt`, `agents-md`, `security-txt`
- robots: `ai-crawler-tokens-robots` (merge into the existing robots.txt; keep its rules)
- if the project exposes an API, dataset, agent or MCP server: `ard` (same bytes also at `/.well-known/ai-catalog.json`)
- if it runs an MCP server: `mcp-server-cards` (SEP-2127; served at `<mcp-url>/server-card`)
- if it has an OpenAPI description: `api-catalog-rfc9727`
- if it is an A2A agent: `a2a`
- if it publishes reusable agent skills: `anthropic-agent-skills-format` (JSON index at `/.well-known/agent-skills/index.json`; each skill is a `SKILL.md`)
- if it documents agent authentication or public access policy: `auth-md` (`/auth.md`)
- if it publishes a structured Markdown concept bundle: `okf` (Open Knowledge Format v0.2 index at `/okf/index.md`)
Replace every placeholder named in `edit_these` with real values from the repository. After deploy, run `validate_manifest` again.

## Compare or explain standards
- `get("<id or file name>")` for one record (accepts `llms.txt`, `/.well-known/ard.json`, `A2A`).
- `search("A2A vs MCP")` to resolve names; `define("<term>")` for glossary terms.
- Use the server's `compare-protocols` prompt, or read `https://agenticweb.wiki/compare/<a>-vs-<b>/`.
- `changes({"since": "<date>"})` for what moved; `stale()` for what has not been re-verified.

## Rules
- Quote URLs and field names from tool output only. Do not invent fields a specification does not have.
- Prefer standards with phase stable/living and observed adoption; say when a pick is a draft.
- Mention the data licence (CC BY 4.0, agenticweb.wiki) when copying its text into a project.
