# CLAUDE.md — DJ Copilot

DJ Copilot (this repo, `ANcpLua/dj-copilot`) is the AISF Agents League **Reasoning Agents
(Foundry)** submission candidate. Built 2026-06-10, fully verified, **on hold pending human
steps** — the code side is DONE.

## What this is

A fork of the MIT-licensed CodeSonify engine (@jpablortiz96, previous league winner) carrying a
new DJ Copilot layer: `src/dj/` (DJSequencer, Camelot wheel, dj-craft KB corpus, Foundry IQ MCP
client, OTel→qyl) + MCP tool `sequence_set` in `src/mcp/index.ts`. Provenance and the
new-vs-inherited boundary are documented in README (top section + mermaid diagram) — keep it
that way; it is the originality defense.

## Verified state (all real runs, 2026-06-10)

- `npm ci && npm run dj` — clean-checkout demo, 11/11 transitions cited, artifacts in `djset-out/`.
- `npm run dj:live-check` — live `knowledge_base_retrieve` over the real KB returns a cited rule.
- Live KB: `dj-craft-kb` on `otel-search` (Azure AI Search, Free tier, $0) — 16 corpus docs
  uploaded. Wiring facts + gotchas: `~/.djcopilot/WIRING.md`. Credentials:
  `set -a && source ~/.djcopilot/foundry-iq.env && set +a` — never print or commit values.
- OTel spans flow to qyl (OTLP :4318) with api-key redaction; verified leak-free.

## Remaining steps (human-owned — do not automate)

1. **Record the demo video** — league rule: it must be *solely Alexander's own work* (filming,
   editing, graphics). Do not reuse any frame of the original author's video.
2. Optional zero-cost de-risk: one line to the organizer (Ayça Baş) confirming the
   MIT-engine-with-attribution foundation is acceptable.
3. **Rotate the Azure keys after the demo is recorded** (regenerate admin + query keys, rewrite
   `~/.djcopilot/foundry-iq.env` — an agent can do this on request).
4. Submit: public repo + ≤5-min video + architecture diagram (already in README). Deadline
   2026-06-14.

## Rules verdict (researched 2026-06-11, don't re-litigate)

Both governing docs were read: the AI Skills Fest T&C PDF covers only the sweepstakes and defers
the hackathon to the Agents League Official Rules; those rules permit licensed third-party
material ("not copied without permission" + "licenses obtained" clause). MIT = permission.
The judged surface (everything in `src/dj/` + the Foundry IQ integration) is original work.

## Working in this repo

- TypeScript ESM, strict, NodeNext (`.js` import suffixes). `npx tsc --noEmit` must stay clean.
- The dj-craft corpus (`src/dj/djCraftCorpus.ts`) is the single source of truth — the
  LocalFoundryIq fixture serves it in-process and `npm run dj:upload-kb` pushes it live.
  Citations ride in-band as `[source: kb/...]`.
- The set must always complete: KB lookup failures degrade to a plain crossfade, never abort.
- Existing CodeSonify engine behavior (5 original MCP tools, web UI) must stay unbroken.
