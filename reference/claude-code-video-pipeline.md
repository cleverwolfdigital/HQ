# Claude Code — AI Video Editing Pipeline

**Type:** SOP
**Category:** Operations
**Source:** YouTube — Claude + HyperFrames Just Solved Video Editing
**Date:** 2026-06-24
**Related:** [[video-generation-tools]] [[claude-code-skills]]

---

## Repos installed into Claude Code (Desktop App)

| Repo | Role |
|---|---|
| **video-use** | Trims raw video — removes filler words, silences, retakes. Outputs edited MP4 + word-level timestamped transcript JSON |
| **HyperFrames** | Adds motion graphics/animations synced to transcript timestamps. HTML-based. Renders final video |

---

## Pipeline

```
Raw video file
  → video-use: transcribe → trim mistakes/silences → edited MP4 + transcript JSON
  → HyperFrames: read timestamps → add motion graphics → render final video
```

Claude Code orchestrates both tools via natural language prompts.

---

## One-liners

- **video-use** — AI video editor: drop in raw file, get clean cut with transcript
- **HyperFrames** — motion graphics engine: liquid glass cards, karaoke subtitles, animations synced to spoken words
- **Remotion** — alternative to HyperFrames (built into video-use), less preferred

---

## Setup

1. Clone both repos into Claude Code project folder
2. Create `.env` and add transcription API key
3. Drop raw video into project, reference it with `@filename`
4. Prompt: *"Use video-use to edit this video, then use HyperFrames to add motion graphics"*

### Transcription options (pick one)
- **11Labs API** — best at finding cut points (preferred)
- **OpenAI Whisper API** — solid default
- **Local Whisper** — free, runs in background

---

## Workflow tips

- Use **plan mode** before building — approve the beat/scene plan before Claude writes HTML
- Use **timeline editor** in HyperFrames dashboard for quick visual tweaks without re-prompting
- Tell Claude to **take screenshots** of each scene so it self-verifies before handing back
- Build **style markdown files** per video type (lesson, short, intro) — reuse them so future videos need minimal prompting
- Token cost for full pipeline: ~238k tokens per video

---

## VS Code vs Desktop App

| | Desktop App | VS Code |
|---|---|---|
| Feel | Less intimidating | Full file visibility |
| File access | No sidebar view | See all assets/projects |
| Use when | Quick runs | Managing project files |
