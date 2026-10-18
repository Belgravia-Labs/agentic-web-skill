# agentic-web skill

An agent skill that makes a web project agent-ready: `llms.txt`, `AGENTS.md`, an ARD / AI Catalog
(`/.well-known/ard.json`), an MCP server card (SEP-2127), an A2A agent card, robots AI-crawler rules,
`security.txt` and an RFC 9727 API catalog. It audits a live domain, generates each file from a
template that passes the validators, and re-checks after deploy.

It does not write these formats from memory. Every lookup, probe, validation and template comes from
the [agenticweb.wiki](https://agenticweb.wiki) MCP server (`https://agenticweb.wiki/mcp`, Streamable
HTTP, read-only, no auth), which tracks 100+ agentic-web standards against their primary sources.

## Install

Claude Code (personal skills):

```sh
git clone https://github.com/Belgravia-Labs/agentic-web-skill ~/.claude/skills/agentic-web
claude mcp add --transport http agenticweb https://agenticweb.wiki/mcp
```

Grok Build:

```sh
grok mcp add --transport http agenticweb https://agenticweb.wiki/mcp
```

then add `SKILL.md` to the project's skills or rules.

Cursor and other MCP clients: add the server, and paste `SKILL.md` into the project rules.

```json
{ "mcpServers": { "agenticweb": { "url": "https://agenticweb.wiki/mcp" } } }
```

## Use

- "Audit example.com for agent readiness" → `validate_manifest`, a grade, every error with its fix.
- "Make this repo agent-ready" → `template` for each file that applies, written to its target path.
- "A2A vs MCP: which do I need?" → `get`, `search`, the `compare-protocols` prompt.

Public report for any domain: `https://agenticweb.wiki/check/<domain>/`
Badge: `https://agenticweb.wiki/badge/<domain>.svg`

## Files

| File | What |
|---|---|
| `SKILL.md` | The skill. Canonical copy is served at https://agenticweb.wiki/static/skill/SKILL.md |
| `server.json` | MCP Registry entry for `wiki.agenticweb/probe` (same file as published to registry.modelcontextprotocol.io) |

## Licence

The skill files are MIT (see `LICENSE`).

Standards data is CC BY 4.0 from [agenticweb.wiki](https://agenticweb.wiki), source in
[Belgravia-Labs/agenticweb-data](https://github.com/Belgravia-Labs/agenticweb-data). Corrections:
https://agenticweb.wiki/submit/ or info@belgravialabs.com.

A [Belgravia Labs](https://github.com/Belgravia-Labs) project.
