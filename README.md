# Gauntlet Cursor kit

Open this folder in Cursor. It contains the build prompt and seven rules; Cursor picks up `.cursor/rules/*.mdc` automatically.

```
PROMPT.md                              paste as the first Agent message
.cursor/rules/gauntlet-domain.mdc      always on: hall, numbers, vocabulary, the six findings
.cursor/rules/nightshift-ux.mdc        always on: worker-first UX rules
.cursor/rules/gauntlet-design-system.mdc   attaches to tsx/css: tokens, components, floor map
.cursor/rules/mock-scenario-engine.mdc     attaches to src/data, fixtures, engine, state
.cursor/rules/verification-ui.mdc      attaches to Prove screen files: coverage strip, certificate
.cursor/rules/nemotron-api.mdc         attaches to backend/: NVIDIA API client, four calls, SSE, caching
.cursor/rules/mockup-qa.mdc            on demand: @mockup-qa.mdc after each phase
```

## Steps

1. Get a free NVIDIA API key at build.nvidia.com. Open the Nemotron models there and note the **exact model IDs** for the largest reasoning model and the smallest fast model; the prompt tells Cursor to paste them into `backend/app/models.py`.
2. Create `backend/.env` with `NVIDIA_API_KEY=nvapi-...` (the prompt also has Cursor create `.env.example`).
3. Install `promtool` (ships with any Prometheus release) and have your legacy plugins ready in `legacy/`. If the real `check_snmp` won't run against your responder, the fallback scripts work too; tell Cursor which you're using.
4. In Cursor: Agent panel → paste `PROMPT.md` → "Follow this prompt. Start with Phase 0 and stop for my review at the end of each phase."
5. Review at each stop. Phases 0–6 give you the full UI on fixtures (demoable on its own). Phases 7–8 add the live Nemotron backend with cache. Phase 9 is QA.
6. Before judging: `make warm` to fill the cache, then set `GAUNTLET_MODE=cached` and test with Wi-Fi off.

## If you're short on time

Do Phases 0–4 and 6 (Checks, Read, Find, Prove on fixtures), then Phase 7 for the `read` and `prove` endpoints only. The Find screen's finding sentences can stay cached; the one-shot-vs-Gauntlet proof must be live at least once on stage.
