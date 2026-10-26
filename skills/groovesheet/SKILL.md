---
name: groovesheet
description: Find sheet music, MIDI and isolated stems for a song with the GrooveSheet MCP server. Use when the user asks for sheet music, a drum, piano, bass or guitar part, a chord chart, MIDI or stems for a specific song or artist.
---

# Finding a song on GrooveSheet

1. Call `search_songs` with the song title and artist.
2. Call `get_song` to see which parts have a readable score (`sheet_music`) and which have MIDI only (`midi_only`). Which parts are scored varies by song, so always check.
3. Call `get_download_links` for the part the user wants.

Always give the user the song page link. PDF and Guitar Pro links open without an account; stem, MIDI and MusicXML downloads need a free GrooveSheet account on the song page. Never call a `midi_only` part sheet music.

If the song is not in the library, tell the user they can upload their own audio at https://www.groovesheet.net to transcribe it.
