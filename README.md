<!--
  Fork notice (Apache License 2.0, Section 4b):
  This README was modified by Dedi Nugroho. The fork-specific sections are
  everything above the "Upstream README" divider and the "Credits & License"
  section at the end. See FORK_CHANGES.md for the full change log.
-->

<p align="center">
  <a href="https://github.com/dedinugroho1/hyperframes"><img src="https://img.shields.io/badge/Hyperframes_Custom_Build-Maintained_by_Dedi_Nugroho-0f172a?style=for-the-badge&logo=github&logoColor=white" alt="Hyperframes Custom Build - Maintained by Dedi Nugroho"></a>
</p>

<p align="center">
  <a href="https://github.com/heygen-com/hyperframes"><img src="https://img.shields.io/badge/fork_of-heygen--com%2Fhyperframes-6e40c9?logo=github" alt="Fork of heygen-com/hyperframes"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" alt="License: Apache 2.0"></a>
  <a href="FORK_CHANGES.md"><img src="https://img.shields.io/badge/modifications-documented-success" alt="Modifications documented in FORK_CHANGES.md"></a>
  <img src="https://img.shields.io/badge/status-unofficial_community_fork-orange" alt="Status: unofficial community fork">
</p>

> [!IMPORTANT]
> **Hyperframes Custom Build — Maintained by Dedi Nugroho**
>
> This repository is an **independent, unofficial fork** of [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes).
> It is **not** an official HeyGen release and is **not affiliated with, sponsored by, or endorsed by HeyGen, Inc.**
> "HyperFrames" and "HeyGen" are names and marks of HeyGen, Inc.; they are used here only to describe where this code comes from.
>
> Every change made in this fork is listed in [`FORK_CHANGES.md`](FORK_CHANGES.md), and the original copyright and license are kept intact in [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).

## About This Fork

HyperFrames turns HTML, CSS, media, and seekable animations into deterministic MP4 video. I forked it to use and extend that rendering core for my own projects, with three focus areas:

| Focus area                          | What it means in this fork                                                                                                                                                                |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Programmatic video rendering**    | Generating videos from data and templates in code rather than by hand, and tuning render settings (resolution, frame rate, concurrency, encoding) for throughput and reproducible output. |
| **Headless pipeline integration**   | Running the CLI and the `@hyperframes/producer` pipeline unattended — from scripts, CI jobs, containers, or backend services — so a video is just another build artifact.                 |
| **Dynamic animation customization** | Driving compositions with variables and runtime data (GSAP timelines, sub-compositions, registry blocks) so one template can produce many tailored videos.                                |

These are the goals of the fork. Work that has actually landed is recorded in [`FORK_CHANGES.md`](FORK_CHANGES.md); anything not listed there behaves exactly as upstream.

## Quick Start (this fork, from source)

The official `hyperframes` npm package and the Claude Code plugin are published by HeyGen from the upstream repository. To run **this fork's** code, build it from source.

### 1. Prerequisites

| Tool                           | Version     | Check with        | Notes                                                                      |
| ------------------------------ | ----------- | ----------------- | -------------------------------------------------------------------------- |
| [Node.js](https://nodejs.org)  | 22 or newer | `node -v`         | Required by the CLI (`engines.node >= 22`).                                |
| [Bun](https://bun.sh)          | latest      | `bun -v`          | The workspace package manager. Do not use npm or pnpm to install the repo. |
| [FFmpeg](https://ffmpeg.org)   | any recent  | `ffmpeg -version` | Encodes captured frames into video.                                        |
| [Git LFS](https://git-lfs.com) | optional    | `git lfs version` | Only needed for the golden regression-test videos (~240 MB).               |

Headless Chrome for frame capture is managed by the toolchain; `hyperframes doctor` (step 4) reports anything missing.

```bash
# macOS (Homebrew)
brew install node ffmpeg git-lfs && curl -fsSL https://bun.sh/install | bash

# Ubuntu / Debian
sudo apt install ffmpeg git-lfs && curl -fsSL https://bun.sh/install | bash   # install Node 22+ via nvm or NodeSource
```

### 2. Clone

```bash
# Source only (fast, skips the large test videos)
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/dedinugroho1/hyperframes.git
cd hyperframes
```

### 3. Install and build

```bash
bun install      # install workspace dependencies
bun run build    # build every package (core, engine, producer, studio, CLI, ...)
```

### 4. Verify your environment

```bash
node packages/cli/bin/hyperframes.mjs doctor
```

### 5. Create, preview, and render a video

```bash
# Scaffold a project outside the repo, using the CLI you just built
node packages/cli/bin/hyperframes.mjs init ../my-video
cd ../my-video

node ../hyperframes/packages/cli/bin/hyperframes.mjs lint      # static checks
node ../hyperframes/packages/cli/bin/hyperframes.mjs preview   # live preview in the browser
node ../hyperframes/packages/cli/bin/hyperframes.mjs render    # render to MP4
```

Tip: add an alias so the fork's CLI is available as `hf`:

```bash
alias hf="node $(pwd)/packages/cli/bin/hyperframes.mjs"   # run from the repo root
```

### Working on the fork itself

```bash
bun run dev       # start the Studio composition editor
bun run test      # run all tests
bun run lint      # oxlint
bun run format    # oxfmt
```

No environment variables are needed for basic use (see [`.env.example`](.env.example)). Contribution workflow, test commands, and conventions are in [`CONTRIBUTING.md`](CONTRIBUTING.md).

### Reporting issues

- A problem **specific to this fork** → [dedinugroho1/hyperframes issues](https://github.com/dedinugroho1/hyperframes/issues)
- A problem that also happens **upstream** → [heygen-com/hyperframes issues](https://github.com/heygen-com/hyperframes/issues)

---

<p align="center"><sub>⬇️ <b>Upstream README</b> — the content below is from <a href="https://github.com/heygen-com/hyperframes">heygen-com/hyperframes</a> and describes the upstream project, its official packages, and its community channels. ⬇️</sub></p>

---

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/logo/dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="docs/logo/light.svg">
    <img alt="HyperFrames" src="docs/logo/light.svg" width="300">
  </picture>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/hyperframes"><img src="https://img.shields.io/npm/v/hyperframes.svg?style=flat" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/hyperframes"><img src="https://img.shields.io/npm/dm/hyperframes.svg?style=flat" alt="npm downloads"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" alt="License"></a>
  <a href="https://nodejs.org"><img src="https://img.shields.io/badge/node-%3E%3D22-brightgreen" alt="Node.js"></a>
  <a href="https://discord.gg/EbK98HBPdk"><img src="https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white" alt="Discord"></a>
</p>

<p align="center"><b>Write HTML. Render video. Built for agents.</b></p>

<p align="center">
  <a href="https://www.hyperframes.dev/studio">Studio</a> |
  <a href="https://hyperframes.heygen.com/quickstart">Quickstart</a> |
  <a href="https://hyperframes.heygen.com/showcase">Showcase</a> |
  <a href="https://www.hyperframes.dev/">Playground</a> |
  <a href="https://hyperframes.heygen.com/catalog/blocks/data-chart">Catalog</a> |
  <a href="https://hyperframes.heygen.com/introduction">Docs</a> |
  <a href="https://discord.gg/EbK98HBPdk">Discord</a>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/f6ff9fae-f33d-4f68-bd54-f3ed4ba6473b" alt="HyperFrames app opening from the Dock: native Mac and Linux app now available" width="800">
</p>

HyperFrames is an open-source framework for turning HTML, CSS, media, and seekable animations into deterministic MP4 videos. Use it locally with the CLI, from AI coding agents with skills, or as the rendering core behind hosted authoring workflows.

## Quick Start

### With an AI coding agent

For Claude Code, install the versioned plugin:

```bash
claude plugin marketplace add heygen-com/hyperframes
claude plugin install hyperframes@hyperframes
```

Enable auto-update for the **hyperframes** marketplace in `/plugin` → **Marketplaces**,
then use `/hyperframes:hyperframes`. See the [plugin guide](docs/guides/plugins.mdx)
for Copilot, VS Code, Cursor, Gemini CLI, updates, and migration.

For standalone skills (including OpenCode), use:

```bash
npx skills add heygen-com/hyperframes
```

> The picker opens with nothing pre-selected — the **Core Skills** group is all you need: the `/hyperframes` router installs each creation workflow on demand. Agents and non-interactive runs should use `npx hyperframes skills update` instead — it installs exactly the core set, whereas `skills add --all` installs all 21 published skills. The six repo-internal skills under `.claude/skills` / `.agents/skills` are excluded by default. For the full published set use `npx hyperframes skills`.
>
> `skills add` resolves the skills.sh registry blob, which can lag `main` by hours. `npx hyperframes skills update` installs from the current `main`, so reach for it when you need the newest copy of a skill.

Try a prompt like:

> Using `/hyperframes`, create a 10-second product intro with a fade-in title, a background video, and subtle background music.

The skills teach agents the HyperFrames production loop: plan the video, write valid HTML, wire seekable animations, add media, lint, preview, and render. They work with Claude Code, Codex, Cursor, Gemini CLI, IBM Bob, and other coding agents that support skills.

## Skills

HyperFrames ships 21 skills agents load on demand. Read `/hyperframes` first — it's the router and capability map; it picks a workflow for any "make me a…" request — video, deck, or composition port — and points to the domain skills below.

For standalone installation, default to the **core set** — the router installs each creation workflow on demand. `npx hyperframes skills update` installs exactly that from anywhere; the interactive picker (`npx skills add heygen-com/hyperframes`) lists it as the "Core Skills" group, nothing pre-selected. The picker is interactive-only — a non-interactive or agent run without `--skill` installs all 21. Use `npx skills add heygen-com/hyperframes --all` to install the complete published set deliberately (skips the picker), or `npx skills add heygen-com/hyperframes --skill <name>` for just one (bare name, no leading `/`).

Standalone installs stay lean after that: `npx hyperframes init` keeps the **core set** fresh (the router, the `hyperframes-*` domain skills, and `media-use` — plus whatever is already installed; `/figma` stays on demand) and never expands a partial install; the creation workflows install **on demand** — the router runs `npx hyperframes skills update <workflow>` before entering one. Nothing re-pulls the full set behind your back.

### Plugin packages

Plugins bundle the full skill catalog and use their agent's update manager.
`bun run package:agent-plugin` builds the committed portable ZIP, source metadata,
and SHA-256 checksum. See the [maintainer checklist](docs/contributing/agent-plugins.mdx).

### Upload to Codex

Build the upload-ready Codex plugin archive from the committed `HEAD` version of the manifest, brand assets, and skills:

```bash
bun run package:codex-plugin
```

This writes `dist/hyperframes-plugin.zip` with a `hyperframes/` root folder and fails if the archive exceeds Codex's 100 MB upload limit.

### Router

| Skill          | Use when                                                                                                                                                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/hyperframes` | **Read first** for any request to make / create / edit / animate / render a video, animation, or motion graphic. Capability map for the domain skills, the intent layer that confirms every creation brief up front, and intent router for the creation workflows below. |

### Creation workflows

| Skill                      | Use when                                                                                                                                                                                                                     |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/product-launch-video`    | Any **website** — marketing / launching / promoting a product (from its URL, a brief, or a script), or a site tour / showcase / social clip featuring the site's own visuals. Up to ~3 min (sweet spot 30-90s).              |
| `/faceless-explainer`      | **Explaining a topic / concept** from arbitrary text — no product, no URL, no website capture; every visual is LLM-invented (typography / abstract / diagram / data-viz).                                                    |
| `/pr-to-video`             | A **GitHub pull request** (PR URL, `owner/repo#N` ref, or "this PR") → changelog / feature-reveal / fix / refactor explainer, read via the `gh` CLI.                                                                         |
| `/embedded-captions`       | Adding **captions / subtitles** to an existing talking-head video (footage untouched) — verbatim rail, embedded climax behind the subject, or pure-cinematic embed.                                                          |
| `/talking-head-recut`      | Packaging an existing talking-head / interview / podcast video with **designed graphic overlays** — lower-thirds, data callouts, kinetic titles, pull-quotes, side panels, PiP.                                              |
| `/motion-graphics`         | A short, **unnarrated, design-led motion graphic** (~under 10s) — kinetic type, stat / chart hit, logo sting, lower-third, animated tweet / headline. MP4 or transparent overlay.                                            |
| `/music-to-video`          | A **music track** (audio file, video to pull audio from, or one generated from a mood brief) → a **beat-synced** video — lyric, slideshow, or kinetic promo; music drives pacing.                                            |
| `/slideshow`               | A **presentation / pitch deck / interactive deck** — discrete slides, fragment reveals, branching, hotspot navigation, presenter mode. Output is a navigable deck, not a rendered video.                                     |
| `/general-video`           | **Anything else** — longer or multi-scene pieces, brand / sizzle reel, title card, static loop, freeform composition. Input- and length-agnostic fallback, and the home of companion mode (co-create with the full toolbox). |
| `/remotion-to-hyperframes` | **Porting an existing Remotion** (React) composition's source to HyperFrames HTML. One-way migration, not creation.                                                                                                          |

### Domain skills (loaded on demand)

Atomic capabilities the creation workflows compose against — pull one when you need that specific layer.

| Skill                    | Covers                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/hyperframes-core`      | The composition contract — `data-*` timing attributes, `class="clip"`, tracks, sub-compositions, variables, framework-owned media playback, determinism rules.                                                                                                                                                                                                                                                                                                                       |
| `/hyperframes-animation` | All animation knowledge — atomic motion rules, scene blueprints, transitions, runtime adapters (GSAP / Lottie / Three.js / Anime.js / CSS / WAAPI / TypeGPU).                                                                                                                                                                                                                                                                                                                        |
| `/hyperframes-keyframes` | Seek-safe keyframe authoring across runtimes — GSAP timelines, CSS keyframes, Anime.js, WAAPI, FLIP, paths, masks, SVG morph/draw, 3D depth — plus `hyperframes keyframes` diagnostics for rendered motion.                                                                                                                                                                                                                                                                          |
| `/hyperframes-creative`  | Non-animation creative direction — `frame.md` / `design.md`, palettes, typography, narration, beat planning, audio-reactive visuals, composition patterns.                                                                                                                                                                                                                                                                                                                           |
| `/media-use`             | The media OS (a host app's own music and sound-effect tools come first for those) — resolve any media need (BGM, SFX, image, icon, logo, voice, color grade, LUT) into a frozen local file or paste-ready block + ledger record, generate via TTS/music/image models when the catalog misses, transcribe, caption, remove backgrounds, and reuse assets across projects. One shared audio engine + manifest tracking.                                                                |
| `/hyperframes-cli`       | CLI dev loop — `init`, `lint`, `check`, `snapshot`, `preview`, `render`, `publish`, `doctor`, plus HeyGen-hosted cloud rendering (`cloud render`) and AWS Lambda rendering (`lambda deploy / render / progress`).                                                                                                                                                                                                                                                                    |
| `/hyperframes-audio`     | Mix the audio already placed in a composition — voiceover carve (dip a music bed only in the bands the voice occupies, static or dynamic, level match included), the effect chain (EQ, compressor, limiter, gate, saturation, delay, reverb, chorus, phaser, bitcrush), automation envelopes on volume or any effect parameter, and submix buses (`<hf-audio-group>`) carrying one chain, fader and automation clock for several tracks at once. Sourcing the audio is `/media-use`. |
| `/hyperframes-registry`  | Search, install and wire registry blocks and components into compositions via `hyperframes catalog` / `hyperframes add`. Load before hand-building any named look, effect, treatment or transition. Authoring a new block or component to contribute upstream.                                                                                                                                                                                                                       |
| `/figma`                 | Import Figma assets, tokens, components, and storyboard sections → reconstructed motion (frames read as states, not slides) (REST/CLI) plus Motion animations (MCP) and shaders (MCP source / native export) into a composition.                                                                                                                                                                                                                                                     |

For visual design handoff workflows, see the [Claude Design guide](https://hyperframes.heygen.com/guides/claude-design) and [Open Design guide](https://hyperframes.heygen.com/guides/open-design).

### Manually with the CLI

```bash
npx hyperframes init my-video
cd my-video
npx hyperframes preview      # preview in browser with live reload
npx hyperframes render       # render to MP4
```

**Requirements:** Node.js 22+, FFmpeg

## What You Can Build

Need ideas? Browse the [Showcase](https://hyperframes.heygen.com/showcase) for finished videos you can watch, read, run, and remix.

- Product launch videos and feature announcements
- PR walkthroughs with animated code diffs, narration, and captions
- Data visualizations, chart races, and map animations
- Social videos with kinetic captions, overlays, and music
- Docs-to-video, PDF-to-video, and site-tour explainers
- Reusable motion graphics for automated content pipelines

## Frame.md

**frame.md — your design system, ready for video.**

Every brand has a `design.md`. None of them were written for a camera. `frame.md` is the missing translation layer: it takes your web-context design spec and inverts it for the frame — the same tokens, the same rules, but rewritten so an AI agent can compose a promo video without guessing at scale or reaching for web chrome.

The output is a `DESIGN.md` superset your whole toolchain can read. Atoms stay sacred. Composition stays free. Numbers come from the script.

<table>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/biennale-yellow"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/biennale-yellow.png" alt="Biennale Yellow" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/biennale-yellow">Biennale Yellow</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/blockframe"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/blockframe.png" alt="BlockFrame" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/blockframe">BlockFrame</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/blue-professional"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/blue-professional.png" alt="Blue Professional" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/blue-professional">Blue Professional</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/bold-poster"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/bold-poster.png" alt="Bold Poster" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/bold-poster">Bold Poster</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/broadside"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/broadside.png" alt="Broadside" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/broadside">Broadside</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/capsule"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/capsule.png" alt="Capsule" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/capsule">Capsule</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/cartesian"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/cartesian.png" alt="Cartesian" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/cartesian">Cartesian</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/cobalt-grid"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/cobalt-grid.png" alt="Cobalt Grid" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/cobalt-grid">Cobalt Grid</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/coral"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/coral.png" alt="Coral" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/coral">Coral</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/creative-mode"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/creative-mode.png" alt="Creative Mode" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/creative-mode">Creative Mode</a></b>
    </td>
  </tr>
</table>

Browse and remix them all at [hyperframes.dev/design](https://www.hyperframes.dev/design).

## How It Works

Define a video as HTML. Add data attributes for timing and tracks. Use GSAP, CSS, Lottie, Three.js, Anime.js, WAAPI, or your own frame adapter for seekable animation.

```html
<div id="stage" data-composition-id="launch" data-start="0" data-width="1920" data-height="1080">
  <video
    class="clip"
    data-start="0"
    data-duration="6"
    data-track-index="0"
    src="intro.mp4"
    muted
    playsinline
  ></video>

  <h1 id="title" class="clip" data-start="1" data-duration="4" data-track-index="1">Launch day</h1>

  <audio
    data-start="0"
    data-duration="6"
    data-track-index="2"
    data-volume="0.5"
    src="music.wav"
  ></audio>

  <script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
  <script>
    const tl = gsap.timeline({ paused: true });
    tl.from("#title", { opacity: 0, y: 40, duration: 0.8 }, 1);
    window.__timelines = window.__timelines || {};
    window.__timelines.launch = tl;
  </script>
</div>
```

Preview instantly in the browser. Render locally or in Docker. The renderer seeks each frame in headless Chrome and encodes the result with FFmpeg, so the same input produces the same video.

## HyperFrames Stack

HyperFrames is the open-source rendering engine, plus a growing set of tools around HTML-native video creation.

| Piece                                           | Status              | What it does                                                                                      |
| ----------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------- |
| CLI                                             | Available           | Scaffold, preview, lint, inspect, and render local video projects                                 |
| Core / Engine / Producer                        | Available           | Parse compositions, drive headless Chrome, encode video, and mix audio                            |
| Catalog                                         | Available           | Reusable blocks and components for transitions, overlays, captions, charts, maps, and effects     |
| Agent skills                                    | Available           | Teach coding agents the video-production patterns that generic web docs miss                      |
| Studio                                          | Available, evolving | Browser surface for previewing and editing compositions                                           |
| AWS Lambda rendering                            | Available           | Deploy a distributed render stack and drive renders from your laptop or CI                        |
| [hyperframes.dev](https://www.hyperframes.dev/) | Available           | Community playground for previewing, iterating, sharing, and rendering HTML-native video projects |
| [frame.md](https://www.hyperframes.dev/design)  | Available           | Invert your design system for the camera — a DESIGN.md superset an agent can compose video from   |

## Catalog

Install ready-to-use blocks and components:

```bash
npx hyperframes add flash-through-white   # shader transition
npx hyperframes add instagram-follow      # social overlay
npx hyperframes add data-chart            # animated chart
```

Browse the catalog at [hyperframes.heygen.com/catalog](https://hyperframes.heygen.com/catalog/blocks/data-chart).

## Why HyperFrames?

- **HTML-native:** compositions are HTML files with data attributes. No React requirement, no proprietary timeline format.
- **Agent-friendly:** agents already write HTML, and the CLI is non-interactive by default.
- **Deterministic:** same input, same frames, same output. Built for CI, regression tests, and automated rendering.
- **No build step:** an `index.html` composition plays as-is and can be previewed directly in the browser.
- **Adapter-based animation:** bring GSAP, CSS animations, Lottie, Three.js, Anime.js, WAAPI, or a custom runtime.
- **Open source:** Apache 2.0 license, with no per-render fees or commercial-use thresholds.

## HyperFrames vs Remotion

HyperFrames is inspired by [Remotion](https://www.remotion.dev). Both tools render video with headless Chrome and FFmpeg. The main difference is the authoring model: Remotion's bet is React components; HyperFrames' bet is plain HTML that humans and agents can both write easily.

|                          | **HyperFrames**                       | **Remotion**                            |
| ------------------------ | ------------------------------------- | --------------------------------------- |
| Authoring                | HTML + CSS + seekable animation       | React components                        |
| Build step               | None; `index.html` plays as-is        | Bundler required                        |
| Agent handoff            | Plain HTML files                      | JSX / React project                     |
| Library-clock animations | Seekable, frame-accurate via adapters | Wall-clock animation patterns need care |
| Distributed rendering    | Local and AWS Lambda render paths     | Remotion Lambda, mature cloud renderer  |
| License                  | Apache 2.0                            | Source-available Remotion License       |

Read the full comparison in the [HyperFrames vs Remotion guide](https://hyperframes.heygen.com/guides/hyperframes-vs-remotion).

## Documentation

Full documentation: [hyperframes.heygen.com/introduction](https://hyperframes.heygen.com/introduction)

- [Quickstart](https://hyperframes.heygen.com/quickstart)
- [Showcase](https://hyperframes.heygen.com/showcase)
- [Guides](https://hyperframes.heygen.com/guides/gsap-animation)
- [API Reference](https://hyperframes.heygen.com/packages/core)
- [Catalog](https://hyperframes.heygen.com/catalog/blocks/data-chart)
- [Examples](https://hyperframes.heygen.com/examples)
- [AWS Lambda rendering](https://hyperframes.heygen.com/deploy/aws-lambda)

## Packages

| Package                                                          | Description                                                       |
| ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| [`hyperframes`](packages/cli)                                    | CLI for creating, previewing, linting, and rendering compositions |
| [`@hyperframes/core`](packages/core)                             | Types, parsers, generators, linter, runtime, and frame adapters   |
| [`@hyperframes/engine`](packages/engine)                         | Seekable page-to-video capture engine using Puppeteer and FFmpeg  |
| [`@hyperframes/producer`](packages/producer)                     | Full rendering pipeline for capture, encode, and audio mix        |
| [`@hyperframes/studio`](packages/studio)                         | Browser-based composition editor UI                               |
| [`@hyperframes/player`](packages/player)                         | Embeddable `<hyperframes-player>` web component                   |
| [`@hyperframes/shader-transitions`](packages/shader-transitions) | WebGL shader transitions for compositions                         |
| [`@hyperframes/aws-lambda`](packages/aws-lambda)                 | AWS Lambda SDK and deployment surface for distributed renders     |

## Community

HyperFrames is used in production at [HeyGen](https://www.heygen.com), with community examples from teams like [tldraw](https://tldraw.com), [TanStack](https://tanstack.com), and others in [ADOPTERS.md](ADOPTERS.md). Open a PR if your team is using HyperFrames.

- Questions and ideas: [Discord](https://discord.gg/EbK98HBPdk)
- Bugs and feature requests: [GitHub Issues](https://github.com/heygen-com/hyperframes/issues)
- User research: [Book a casual 30-minute conversation with the HyperFrames team](https://calendar.google.com/calendar/u/0/appointments/schedules/AcZssZ2cSpKoDgmcmRrgekrnrgqmvPT8W6F2Zg6e7MY7IJqaZKwpn_I0NdTHkN390iguMepE_NVg8ezb?gv=true) — no preparation or sales pitch
- Security reports: [SECURITY.md](SECURITY.md)
- Contributions: [CONTRIBUTING.md](CONTRIBUTING.md)

## Development Note

The repo uses [Git LFS](https://git-lfs.com) for golden regression-test baselines under `packages/producer/tests/**/output.mp4` (about 240 MB of `.mp4` files). If you're cloning the full repo for development, install Git LFS first:

```bash
# macOS
brew install git-lfs

# Ubuntu / Debian
sudo apt install git-lfs

# Windows
winget install GitHub.GitLFS

# Then, once per machine
git lfs install
```

If you only need source files, you can skip LFS content:

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/heygen-com/hyperframes.git
```

## Credits & License

**Hyperframes Custom Build** is a derivative work of **[HyperFrames](https://github.com/heygen-com/hyperframes)**, created and maintained by **[HeyGen, Inc.](https://www.heygen.com)** and its contributors. All credit for the original framework belongs to them.

- **Upstream project:** [github.com/heygen-com/hyperframes](https://github.com/heygen-com/hyperframes)
- **Original copyright:** Copyright 2026 HeyGen, Inc.
- **License:** [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0); the full, unmodified text is in [`LICENSE`](LICENSE)
- **Attribution notices:** [`NOTICE`](NOTICE)
- **Fork modifications:** listed file by file in [`FORK_CHANGES.md`](FORK_CHANGES.md); modifications are Copyright 2026 Dedi Nugroho and released under the same Apache 2.0 license
- **Third-party acknowledgements:** [`CREDITS.md`](CREDITS.md)

This fork is not an official HeyGen product. "HyperFrames" and "HeyGen" are names and marks of HeyGen, Inc. and are used here only to identify the origin of the code, as permitted by Section 6 of the Apache License 2.0. The software is provided "AS IS", without warranties or conditions of any kind; see Sections 7 and 8 of the license.

Fork maintained by **Dedi Nugroho** · [github.com/dedinugroho1](https://github.com/dedinugroho1)
