# What the existing Borumi work tells us

Read-only audit, 2026-10-16. This report contains aggregate findings only. Project names, transcripts, media, local paths, screenshots, and source-specific claims are excluded.

## Evidence and scope

The audit inventoried 58 local project bundles across the main project folder and one mounted archive. It inspected metadata in 13 representative bundles, including raw recordings, edited tutorials, and reel batches. Five edited tutorials contained 74 scenes and 3,175 timeline segments. Their running times ranged from about 14 to 27 minutes, with 289–934 segments and 84–299 stored commits per project.

Twenty frames were sampled from three existing long-form exports and one vertical reel. The reel was verified as 57.1 seconds, 1080×1920, 30 fps; long-form samples were 60 fps. Four existing research packs and their animation folders were counted. The running editor's project overview and existing bridge logs were also read. No project was opened, switched, edited, exported, or published by this audit. Sampling is evidence of the visual patterns below, not a full playback or audio-quality review.

## The repeatable visual system

The long-form samples consistently use a dark dot-grid canvas, a large proof or screen-recording panel on the left, a camera tile on the right, and a small section badge above the camera. They alternate between real app captures, highlighted source material, restrained diagrams, and short chapter cards. Five edited projects contained 191 explicit layouts: 161 custom, 29 fullscreen, and one side layout.

The sampled reel uses a different composition: full-height camera, large screenshot cards, blurred backing, large yellow-and-white captions, a thin progress bar, and a closing call to action. The file identifies Remotion as its renderer. The original reel composition source was not located; finding a rendered file does not establish an editable template.

These patterns support a small reusable template library:

| Template | Required inputs | Deterministic work |
|---|---|---|
| Source proof panel | Verified capture, crop, highlight, source URL and capture date | Crop, fit, label, highlight, restrained motion |
| Screen demonstration | Original screen clip, source range, pointer or zoom cue | Trim, scale, position, privacy mask |
| Concept diagram | Verified labels, relationships, step state | Reuse diagram components and transitions |
| Chapter and step badge | Short title, section number, timing | Render from fixed typography and geometry |
| Vertical reel | Selected source ranges, camera/screen mapping, caption words, hook and CTA | Reframe, compose cards, animate captions, render progress |

Most sampled visuals do not require paid generative video. Use image/video generation when the brief actually needs an imagined scene or a missing illustrative shot. Generated imagery cannot substantiate a factual claim.

## Measured sources of repeated work

### Research context

The four research packs held 47, 83, 90, and 96 entries. Their JSON indexes ranged from 64 KB to 272 KB. The largest index plus its prose README occupied about 569 KB, with overlapping information in both files. They referenced 234 distinct exact URL strings; 19 URLs appeared in multiple projects. The four proof folders held 1,712 image/video asset files, including alternate crops, highlights, and contact sheets—not 1,712 unique sources.

Implication: create one normalized source ledger, then send workers only the claims, sources, and assets for their assigned scene. A worker does not need the entire proof README and its equivalent JSON. Cache original captures separately from derived crops and highlights. Recheck changing facts such as prices and counts according to the new video's date; a cached screenshot is not a fresh verification.

### Renders

The same four projects contained 73 animation jobs and 166 versioned animation video files, about 2.27 saved versions per job. Version counts do not show which revisions were necessary, how long rendering took, or what they cost.

Implication: lock the layout and data before rendering full-resolution motion. Check an inexpensive still or contact sheet first, followed by a low-resolution motion preview. Cache renders using the composition source, assets, fonts, timing, renderer version, and output settings. Change only the affected section when revising a fact, label, or crop.

### Local editor reads

A bounded snapshot of the existing bridge log contained 229,992 calls over its recorded week. Of these, 201,576 (87.6%) were `get_timeline`, 10,351 were `inspect_timeline`, 6,351 were `get_scenes`, and 4,037 were `get_transcript`. An identical timeline-summary request appeared 2,408 times. These are local API calls. They are **not** model requests, token usage, billed spend, or evidence of a corresponding reduction in subscription usage.

The inspected writer takes repeated project-wide snapshots. Each snapshot independently fetches layout, overlay, content, and control layers through windowed timeline reads. Those categories can share one windowed read and be separated in local code. When all four categories are present, an illustrative 24-minute snapshot with eight 180-second windows requires roughly 38 timeline/scene calls in the current pattern versus 10 for one combined pass. That is an implementation-level request-count estimate, not a measured end-to-end speedup.

Keep the pre-edit, staged-edit, collision, and post-commit checks. Cache within a verified project revision or transaction mutation epoch, never across unknown edits. Batch local reads in one script and return a compact summary to the model. Reuse completed snapshots when the revision is unchanged.

## Data contracts that a cheaper model still needs

The project database is SQLite inside a project bundle. Recorded word timings are already present in media transcription JSON, with the sampled files identifying the Parakeet engine. Reuse these timings before paying to transcribe again. They are source-media times in seconds; timeline tools use project times in milliseconds. A source word needs clip offset, playback rate, take, scene position, and edit mapping before it becomes an edited-video caption.

Two real-data pitfalls matter:

- Some segment durations are null. Treat indefinite segments explicitly.
- An overlay may span beyond its owning scene. It must not increase the scene's duration. In one inspected project, naively summing the maximum end of every segment produced 1,549.8 seconds; the live editor and content-only calculation both gave 905.2 seconds. Use the editor's scene table as authoritative when available.

Session aliases are connection-scoped. Obtain fresh project and segment IDs from the same connection used for the operation. A raw duplicate's name alone does not prove it backs up all of a newer project: one short raw copy coexisted with a later full-recording copy. Match media provenance and source coverage.

## Recommended work order

1. A local script reads one project snapshot, existing transcripts, source paths, and prior receipts. It builds a scene manifest and identifies the current revision.
2. A capable editor chooses the narrative and protected material once. A cheaper worker proposes cuts using a bounded scene transcript and explicit rules. Only ambiguous cuts escalate.
3. Independent research workers handle distinct claim groups. They share a normalized source ledger and return small structured records. One verifier reviews unsupported claims, numeric claims, consequential claims, and source/capture mismatches.
4. Workers choose and populate existing visual templates. Generate custom code or paid media only for a missing capability. Run CPU-heavy renders with bounded concurrency.
5. One writer stages and applies edits. Scene workers never compete to mutate the live timeline. Rendered frame checks, revision checks, and rollback receipts remain mandatory.
6. Export the final master once after the edit is stable. Build reel candidates from its source-range map and original camera/screen assets. Reusing research, captions, and source footage saves work; cropping a finished side-by-side master does not recreate the observed vertical reel.
7. Check changed sections and every edit boundary, then complete a final output review. Carry unresolved factual and privacy issues into the handoff. Do not silently turn an unsupported spoken claim into a sourced claim by adding a graphic.

No model comparison or subscription-quota experiment was run during this audit. The evidence supports reducing repeated context, repeated research, and repeated rendering while preserving verification; exact savings require measuring the same scene under each workflow.
