[ViralForge-AI-README.md](https://github.com/user-attachments/files/31337017/ViralForge-AI-README.md)
# ViralForge AI

**The Autonomous AI Creative Director** — give it a topic, and it discovers the opportunity, researches it, understands it, chooses the creative strategy, generates three full videos, judges them, learns from the failures, and delivers the best publish-ready 9:16 social video.

Built for **{Tech: Europe} × VEED Hackathon — The Summer Lock-In**.
Team: **ViralForge AI** · Builder: **Hossam Elshahaby**
* **Main App:** [ViralForge AI](https://adaptive-media-lab.lovable.app) 
* **Other App:** [ViralForge AI](https://aistudio.google.com/apps/5520094d-4a0c-4b19-98b1-a3bbe688d1ae?showPreview=true&showAssistant=true&project=gen-lang-client-0082911806) 
* **Demo video:** [2 min Video link](https://youtu.be/MoEDxbKMYCE) 

---

## What it does

One click ("Forge the video") drives a 10-step autonomous pipeline, visualized live in a 3D mission-control interface:

| # | Stage | Service | Output |
|---|-------|---------|--------|
| 1 | **Discover** | Tavily web search | Rising stories matched to the topic + creator interests |
| 2 | **Understand** | GLiNER2 (`fastino/gliner2-large-v2` via Hugging Face) + Pioneer | A Trend Knowledge Graph, rendered in 3D |
| 3 | **Direct** | Pioneer (Social Video Intelligence Model) | Story strategy + creative verdict |
| 4 | **Hook** | Pioneer | 3 competing hooks |
| 5 | **Generate media** | fal Media OS | Hero image, 3 B-roll clips, voiceover, music, captions |
| 6 | **Multiply** | — | 3 complete video variants (A / B / C) playing in a 3D theatre |
| 7 | **Evaluate** | Pioneer judge | A/B/C scores across hook, story, visuals, CTA + verdict |
| 8 | **Assemble** | VEED Subtitles API & Fabric 1.0 — **via fal** | Winner captioned and rendered |
| 9 | **Deliver** | — | Publish-ready 9:16 vertical video with narration + music |

### Requirement highlights

- **Pioneer — Social Video Intelligence Model.** Predicts and improves social-video quality. An in-app **Synthetic Fine-Tune Lab** generates thousands of GOOD vs BAD (hook / story / CTA / scene) training pairs and demonstrates the benchmark: **general 0.71 → fine-tuned Pioneer 0.89 → frontier 0.91**.
- **fal as the Media OS.** One API key fans out every media job through fal's queue:
  - Image: `fal-ai/z-image/turbo`
  - Video: `fal-ai/flux-3/text-to-video/draft` (audio-enabled, up to 20s)
  - Voice: `fal-ai/kokoro/american-english`
  - Music: `CassetteAI/music-generator`
  - VEED: `veed/subtitles`, `veed/fabric-1.0` — **VEED is accessed entirely through fal** (auth + billing via the fal key).

## Tech stack

- **App:** TanStack Start v1 (React 19, SSR), Vite, Tailwind CSS v4, shadcn-style tokens
- **3D:** React Three Fiber + drei, Zustand store, camera choreography per stage
- **AI:** Lovable AI Gateway (`google/gemini-3.7-flash`) powering the Pioneer intelligence layer
- **Search:** Tavily · **Entities:** GLiNER2 on Hugging Face Inference
- **Media:** fal.ai queue API (image / video / audio / VEED)

## Project layout

```
src/
  routes/index.tsx               # 3D command center route
  components/scene/              # R3F scene: knowledge graph, variant theatre, camera rig
  components/hud/                # Mission-control HUD: top bar, log stream, stage panel, synthetic lab
  lib/
    ai-gateway.server.ts         # Lovable AI Gateway provider
    pioneer.server.ts / pioneer-core.server.ts / pioneer.functions.ts
    tavily.functions.ts          # Trend discovery
    gliner.server.ts / extract.functions.ts
    fal.server.ts / fal.functions.ts
    veed.server.ts / veed.functions.ts
    pipeline/                    # Zustand store, orchestrator, demo data, fallback assets
public/
  fallback-final.mp4             # Bundled narrated 9:16 demo cut
  fallback-voice.mp3             # Bundled neural voiceover
```

## Configuration

All keys are **server-side secrets only** — never exposed to the browser:

| Secret | Purpose |
|--------|---------|
| `FAL_KEY` | fal.ai (image/video/audio + VEED Subtitles & Fabric 1.0) |
| `TAVILY_API_KEY` | Trend discovery |
| `HF_TOKEN` | GLiNER2 entity extraction |
| `PIONEER_API_KEY` | Pioneer service (when called directly) |
| `H_API_KEY` | H Company agent |
| `LOVABLE_API_KEY` | Lovable AI Gateway (Pioneer intelligence layer) |

> **Demo resilience:** every live stage ships an on-topic bundled fallback (health-training × humanoid-robotics media), so the full story completes even if an API is unreachable or out of credit. fal API calls require balance — the dashboard's daily free generations apply to the web playground only.

## Run it

```bash
bun install
bun run dev    # http://localhost:8080
```

Open the app, optionally set a **topic seed** and **creator interests / social profiles** (e.g. `@helshahaby — health training with humanoid robotics and AI`), and press **FORGE THE VIDEO**. Use the HUD toggle to hide all panels and watch the 3D pipeline unobstructed.
