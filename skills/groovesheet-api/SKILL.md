---
name: groovesheet-api
description: Build against the GrooveSheet REST API, SDK or CLI from code: search the sheet music library, transcribe audio to sheet music, MIDI and stems, poll jobs and download results, starting in the no-charge sandbox. Use when writing a program or script that needs GrooveSheet, as opposed to connecting a chat app to its MCP server.
---

# GrooveSheet from code

The REST API behind groovesheet.net, its TypeScript SDK and the `groovesheet`
command. Read the OpenAPI description first: https://api.groovesheet.net/openapi.json
(the `info.description` explains auth, the 202 job pattern, Idempotency-Key,
rate limits and the deprecation policy). Human guide:
https://www.groovesheet.net/developers/docs/api

## Start in the sandbox

`https://api.groovesheet.net/sandbox` is the second `servers` entry. It needs
no token and charges nothing: read routes mirror production, and
`POST /sandbox/workflow/{workflow_name}` simulates a job that completes in
about 15 seconds with the real outputs of a demo song (Easy On Me by Adele).

```bash
npx @groovesheet/sdk --sandbox transcribe any.mp3 --instrument drums --wait --out ./out
```

## The loop in production

1. Search: `GET /library/tracks?q=<words>` (no token), or `search_songs` over MCP.
2. Start: `POST /workflow/{workflow_name}` with multipart `file` and
   `Authorization: Bearer <token>`; answers `202` with `workflow_id` and `status_url`.
   Send `Idempotency-Key: <uuid>` so a retry never starts, or charges for, the job twice.
3. Poll: `GET /workflow/status/{workflow_id}` every few seconds until `status`
   is `completed` or `failed`. Many jobs: `POST /workflow/status/batch`.
4. Download: `outputs.files` maps file keys to storage paths; fetch each with
   `GET /workflow/download/{workflow_id}/{file_key}`, or the zip at `/workflow/download/{workflow_id}`.

Workflow names by instrument: drums `separate_to_drumscore_v2_full`, piano
`separate_to_piano_score_full`, bass `separate_to_hfmt_bass_score_full`,
guitar `separate_to_guitar_score_full`, chords `chords_from_mix_full`, stems
`bs_roformer_separate`. `GET /workflows` lists them all.

## SDK

```ts
import { GrooveSheet } from '@groovesheet/sdk';
const gs = new GrooveSheet({ token: process.env.GROOVESHEET_TOKEN });
const job = await gs.transcribe({ file: 'song.mp3', instrument: 'drums' });
const done = await gs.waitForJob(job.workflow_id);
await gs.saveOutputs(done.workflow_id, './out');
```

`new GrooveSheet({ sandbox: true })` for the sandbox. Source:
https://github.com/groovesheet/groovesheet-sdk

## Tokens

OAuth 2.1 with PKCE; dynamic client registration is open at
`https://api.groovesheet.net/oauth/register`. An agent with no browser uses
verified-email registration (`POST /agent/identity`, then poll the token
endpoint with the claim grant); the CLI does this with
`groovesheet login --email you@example.com`. Everything is written out at
https://www.groovesheet.net/auth.md. There is no separate API key.

## Keep straight

- A full song in production spends the account's minutes; the library, the
  sandbox and the 30-second preview are free.
- Every response carries `RateLimit-*` headers; a `429` carries `Retry-After`.
- Deprecated routes answer with `Deprecation` and `Sunset` headers at least
  90 days before they go.
