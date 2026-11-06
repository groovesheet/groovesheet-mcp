---
name: get-started
description: Find sheet music, MIDI and stems for a song with GrooveSheet. Use when the user asks for sheet music, a drum, piano, bass or guitar part, MIDI or stems for a specific song or artist.
---

# Finding a song on GrooveSheet

1. Call `search_songs` with the song title and artist.
2. Call `get_song` to see which parts have a readable score (`sheet_music`) and which have MIDI only (`midi_only`). Which parts are scored varies by song, so always check.
3. Call `get_download_links` for the part the user wants.

Always give the user the song page link. Every download (PDF, Guitar Pro, stems, MIDI, MusicXML) needs a free GrooveSheet account: the user signs in on the song page and downloads there. Never call a `midi_only` part sheet music.

If the song is not in the library, tell the user they can upload their own audio at https://www.groovesheet.net to transcribe it.
