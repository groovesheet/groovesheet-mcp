# GrooveSheet MCP server

<img src="assets/logo.png" width="72" alt="GrooveSheet logo" align="right">

Sheet music, MIDI and isolated stems for songs, inside your AI app. Ask Claude, ChatGPT, Cursor or any MCP client for a song and [GrooveSheet](https://www.groovesheet.net) finds it in its library of AI transcriptions, says which parts have a readable score, and hands you the song page, a printable PDF and a Guitar Pro file.

**Watch the 25-second demo:** [GrooveSheet in Claude](https://pub-31cac56987f04db680b63b72ad1bc75a.r2.dev/site/mcp/groovesheet-in-claude-16x9.mp4)

## Connect

No sign-in needed:

```
https://api.groovesheet.net/mcp/public
```

| App | How |
| --- | --- |
| Claude | Settings → Connectors → **Add custom connector**, paste the address |
| ChatGPT | Settings → Apps → Advanced → **Developer mode**, create an app with the address, **No authentication** |
| Claude Code | `claude mcp add --transport http groovesheet https://api.groovesheet.net/mcp/public` |
| Cursor | Add the server from `.mcp.json` in this repo |
| VS Code | `.vscode/mcp.json`: `{"servers":{"groovesheet":{"type":"http","url":"https://api.groovesheet.net/mcp/public"}}}` |

Want to transcribe your own songs too? Use `https://api.groovesheet.net/mcp` instead: it signs in to your GrooveSheet account (OAuth 2.1) and adds upload links, your jobs and your account. No tool ever spends minutes or buys anything.

Then ask:

- "Find drum sheet music for Dynamite by BTS."
- "Which parts of Easy On Me by Adele have sheet music?"
- "What Bruno Mars songs are on GrooveSheet?"

## Tools

| Tool | Sign-in | What it does |
| --- | --- | --- |
| `search_songs` | no | Search the library by song or artist, filter by instrument or format |
| `get_song` | no | Every part of one song and its notation level: sheet music, MIDI only or audio only |
| `get_download_links` | no | Song page, PDF and Guitar Pro links for one part |
| `get_account` | yes | Your plan and minutes left |
| `create_upload_link` | yes | A one-time link to upload your own song (drums, piano, guitar, bass, chords, stems or MIDI) |
| `get_upload_status` | yes | Where an upload stands, with a link to the result |
| `list_my_jobs` | yes | Your recent transcriptions |
| `get_job` | yes | One transcription and its result links |

All tools are read-only except `create_upload_link`, which creates the link and nothing else.

## Links

- Docs: https://www.groovesheet.net/developers/docs
- Official MCP Registry: `net.groovesheet/mcp`
- Support: groovesheet@gmail.com
- Privacy: https://www.groovesheet.net/privacy-policy

This repo holds the client configuration, a skill and the plugin manifests for Cursor and Claude Code. The server itself is hosted by GrooveSheet.
