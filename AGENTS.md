# AGENTS.md

Instructions for AI coding agents (Claude Code, Codex, Cursor, Windsurf,
Copilot, Gemini CLI and the rest) working in this repository or using
GrooveSheet from a project of their own.

## What this repository is

Client configuration for the GrooveSheet MCP server, a skill, and plugin
manifests. The server itself is hosted by GrooveSheet at
`https://api.groovesheet.net/mcp/public` (no sign-in) and
`https://api.groovesheet.net/mcp` (OAuth 2.1, the user's account). Nothing
here runs a server, builds, or needs dependencies.

Layout:

- `plugin.json`, `mcp.json`, `skills/`: an Agent Plugins package
  (https://agent-plugins.org). `mcp.json` names the public server;
  `skills/groovesheet/SKILL.md` is the skill.
- `.mcp.json`: the same server for Claude Code and Cursor
  (`claude mcp add --transport http groovesheet https://api.groovesheet.net/mcp/public`).
- `.claude-plugin/`, `.cursor-plugin/`, `lhm.plugin.json`: client-specific
  manifests for Claude Code, Cursor and LobeHub. Keep their tool lists in
  step with the server's `tools/list`.
- `skills.sh.json`: how the skill is grouped on https://skills.sh.
- `README.md`: the human setup guide.

## Rules for changes here

- The server's tools are the source of truth. Before editing a manifest, call
  `tools/list` on `https://api.groovesheet.net/mcp/public` (3 tools) and, if
  you have a token, on `https://api.groovesheet.net/mcp` (8 tools). Never
  list a tool the server does not have, and copy descriptions verbatim.
- Keep `plugin.json` within the Agent Plugins 1.0.0 schema: only the fields
  it allows at the top level; anything else goes under `extensions`.
- Bump `version` in `plugin.json` and `package.json` together.
- No em dashes in any file. No prices or claims that are not on
  https://www.groovesheet.net/pricing or in https://www.groovesheet.net/llms.txt.
- Do not add secrets, tokens or account details. The public server needs none
  and the account server uses OAuth with dynamic client registration.

## Using GrooveSheet from your own code

- Everything an agent needs to know, in order: https://www.groovesheet.net/llms.txt,
  https://www.groovesheet.net/agents.md, https://www.groovesheet.net/auth.md.
- REST API: OpenAPI 3.1 at https://api.groovesheet.net/openapi.json. Start
  against the sandbox, `https://api.groovesheet.net/sandbox`, which needs no
  token and charges nothing; docs at https://www.groovesheet.net/developers/docs/api.
- SDK and CLI: `npm i @groovesheet/sdk` (`npx @groovesheet/sdk search "easy on me"`),
  source at https://github.com/groovesheet/groovesheet-sdk.
- Documentation over MCP: `https://www.groovesheet.net/mcp/docs`
  (`search_docs`, `get_doc`, `list_docs`), and a NLWeb `/ask` endpoint at
  https://www.groovesheet.net/ask.
- Facts to keep straight: only parts a song page or `get_song` lists as
  `sheet_music` have a readable score; free means a 30-second preview; tools
  never spend the user's minutes; downloads need a free account.
