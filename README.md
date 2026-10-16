# Video Edit Fast

A portable skill for **Codex, Grok Build, Claude Code, and other local agent harnesses** to edit Borumi videos, research real proof visuals and tweets, and produce reels with less repeated context and work. **Quality stays the acceptance gate; subscription usage and waiting are the optimization targets.**

This is a new companion to [proof-first-video-editor](https://github.com/Samin12/proof-first-video-editor), preserving its Borumi transaction writers and visual style. It replaces the Claude-specific orchestration with small local job packets, scripts, cached assets, and resumable tasks. It does not require another paid service to start.

## Install and use

[Download the skill ZIP](https://github.com/Samin12/proof-first-video-editor-portable/releases/latest/download/video-edit-fast.zip) and extract its `video-edit-fast` folder into `~/.agents/skills/`, or use the clone-based installer below. The downloaded folder includes the same setup script; run its `scripts/setup.py fetch` once before Borumi editing.

```bash
git clone https://github.com/Samin12/proof-first-video-editor-portable.git
cd proof-first-video-editor-portable
python3 skills/video-edit-fast/scripts/setup.py install
python3 skills/video-edit-fast/scripts/setup.py fetch
python3 skills/video-edit-fast/scripts/run.py doctor
```

The skill is linked into `~/.agents/skills/video-edit-fast`, a shared location supported by Codex and Grok Build. Keep the checkout and refresh/restart skill discovery. The fetch step gets pinned public dependencies into an isolated directory; it preserves existing installations and video projects.

In **Codex**:

```text
$video-edit-fast Edit my Borumi video "Project Name". Tighten the intro,
use real screenshots and tweets for the claims, and make three reel candidates.
Keep the established quality, minimize repeated work, and resume from saved work.
```

In **Grok Build**:

```text
/video-edit-fast Edit "Project Name" in Borumi. Start with only the intro.
Keep my visual style. Use two workers at most and save a handoff for Codex.
```

For **Claude Code**, run `setup.py install --harness claude`, then use `/video-edit-fast`. Other local harnesses can read [SKILL.md](skills/video-edit-fast/SKILL.md) and use the same scripts. A browser chat without access to the Mac can prepare research/plans but cannot directly edit local Borumi. [Harness setup and compatibility](skills/video-edit-fast/references/harnesses.md).

## How this saves time and usage

| Work | Default route |
|---|---|
| Inventory, timing, hashes, subtitles, render preparation | Local deterministic scripts |
| Simple extraction and candidate selection | One scene packet; smaller model only after quality checks |
| Story, ambiguous edits, final judgment | Balanced/strong model only where needed |
| Research | Existing source cache first; targeted search/capture for missing claims |
| Visuals | Reusable local templates; real source captures |
| Generated shots | Optional Higgsfield or another verified provider within a set budget |
| Parallelism | Two independent workers initially; one Borumi writer |
| Continuation | Saved goal, scene state, artifacts and hash-checked receipts |

Choose **fast** for targeted cuts, **proof** for full evidence-led production, and **reel** for vertical derivatives. Existing timed transcripts are reused. Completed sources and renders are reused when their inputs remain valid. A small edit does not trigger an entire audio/research/visual pipeline.

Parallelism reduces waiting only where tasks are independent; it does not create more subscription allowance. The priority is to reduce consumed plan capacity per accepted edit, not to choose the cheapest model at any cost. Optional extra paid jobs default to **$0** until a budget is supplied; that setting is a billing guard, not the quality target. Model speed modes and API authentication can change which allowance or bill is used.

Start with the [quota-efficient production workflow](skills/video-edit-fast/references/quota-workflow.md). Read the [in-depth cost and service research](research/services-and-costs.md) for dated official sources, model-routing examples, Firecrawl/Higgsfield/Eden tradeoffs, and an apples-to-apples benchmark. Example API arithmetic is not a promise about your subscription bill or video quality.

## What's included

- [The skill](skills/video-edit-fast/SKILL.md), with references loaded only for the active stage.
- `run.py`: mode/scoped scene packets, advisory task graph, resumable receipts, content cache, and explicit cost arithmetic.
- `setup.py`: install, isolated pinned toolchain, and correctly configured calls to existing Borumi scripts.
- `reel.py`: frame-mapped rough reels and optional SRT remapping from a reviewed local master.
- [Styled reel template](skills/video-edit-fast/assets/reel-template/): reusable local composition for camera/proof cards, captions, progress, and CTA.
- [Borumi study](research/borumi-patterns.md) and [validation record](research/validation.md).

The job runner is a state helper, **not an autonomous model scheduler or a hard spending limiter**. Your harness carries out its ready tasks. Structural checks are not visual QA; the final export must be watched and heard. [Job protocol](skills/video-edit-fast/references/jobs.md), [QA](skills/video-edit-fast/references/qa.md).

## Requirements and boundaries

Python 3.9+ for helpers; FFmpeg/ffprobe for media; macOS with Borumi and MCP enabled for timeline edits. The inherited writers need their documented image/audio dependencies. Styled motion uses Remotion and the existing animation kit; install that only when needed. Service connections and authentication are configured separately in each harness.

The isolated legacy bridge is patched to decline automatic consent prompts and uses port 47812. Timeline changes still go through a backup, staged transaction, live hash check, frame inspection, and committed readback. No private footage, transcripts, screenshots, account data, or credentials are included in this repository. The skill does not publish your videos automatically.

## Test

```bash
python3 -m unittest discover -s tests -v
python3 skills/video-edit-fast/scripts/run.py --help
```

Exact measured validation and remaining limits are in [validation.md](research/validation.md). Lower-cost model quality equivalence has not been established by a controlled full-video benchmark.

MIT. Builds on Samin Yasar's [proof-first-video-editor](https://github.com/Samin12/proof-first-video-editor), [edit-borumi-video](https://github.com/Samin12/edit-borumi-video), and [opencutagent-borumi](https://github.com/Samin12/opencutagent-borumi).
