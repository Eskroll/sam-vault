# The Fixer Files — Production Bible

Reference doc for the POV YouTube series. Copy the "New Chat Starter Prompt" section at the bottom into a fresh Claude chat to pick up production on the next video.

## Series concept

- Format: POV-style dark business thriller, illustrated (not photoreal), narrated in second person ("you").
- Recurring character: "the Fixer," unnamed, called in by wealthy families/companies to handle crises before they become public. Each video is a standalone case, connected by a recurring cold-open/close hook: a call at 2am to start, and a new unnamed incoming call to end, seeding the next episode.
- Tone/inspiration comp: Mentor POV (@MentorPOV) — dark business stories, POV framing, 15-40 min runtime.
- Fictional companies used so far: Meridian Industries (industrial manufacturing conglomerate, video 1).

## Character reference — "the Fixer"

- Man, mid-30s, short dark curly hair, stubble.
- Wears a long dark coat over a charcoal button-up shirt.
- Calm, guarded, controlled demeanor — lets people talk themselves into revealing things rather than confronting directly.
- **Consistency method**: every image prompt attaches the earlier reference image(s) (thumbnail + best prior scene) and includes the line: "Using the attached reference image(s) as the character reference — same face, same dark coat, same build, same hairstyle."

## Visual style anchor (use in every image prompt)

> Flat 2D digital illustration style, cinematic thriller mood, muted cold blue-gray palette with selective warm lighting, bold shadows, slightly desaturated, graphic-novel aesthetic.

16:9 widescreen composition for all scene images. Square 1:1 for profile picture only.

## Production pipeline (per video)
## Production pipeline (per video)

1. **Premise + outline** — pick a new crisis/industry, sketch cold open → discovery → confrontation → resolution → new-call hook.
2. **Script** — full narration written in second person, ~2,800–3,200 words for an 18–20 min runtime at ElevenLabs' natural pace. Delivered as clean prose, no scene labels, ready to paste directly.
3. **Scene/image prompts** — break the script down beat-by-beat, not scene-by-scene. A single scene (e.g. a boardroom confrontation) often contains 3–4 distinct visual beats: arrival, exchange, turn, aftermath. Add a separate prompt whenever a passage has a dialogue exchange over ~150 words, a named shift in power/dynamic, a quiet aftermath beat between two bigger scenes, or an embedded flashback (flashbacks need two images minimum — entering the memory and returning to the present — see Lessons learned). Original rough estimate was 20–25 images; real counts tend to land closer to 28–32. Each prompt is built from the visual style anchor + character reference line + scene-specific description. Any character who reappears — not just the Fixer — needs their reference image attached to every later prompt they're in, not just the protagonist's.
4. **Voice (ElevenLabs)** — Creator plan, Simeon voice ("Reassuring, Deep, and Smooth"), Eleven Multilingual v2 model. Use **Studio** (not the basic Text to Speech tool) to avoid the 5,000-character truncation limit. Export as single MP3 file. Creator plan = 100,000 characters/month; a full episode script (~17,000 characters) is ~17% of that, supporting roughly 5–6 full episodes/month if that's all the plan is used for.
5. **Assembly (CapCut)** — audio track first, images laid in over it in story order, pan/zoom (Ken Burns) animation applied per image, no captions (deliberate style choice), no music yet (may revisit for future videos). For fades: apply Fade In/Fade Out animation only to the very first and very last image of the whole video — fading every single image to black between cuts reads as a slideshow, not cinematic. Use a cross-dissolve **Transition** between every other cut instead (set it once between any two clips, then click "Apply to All" in the transitions panel to apply it to every cut on the timeline in one action). To batch any per-clip animation/filter across many clips: set it on one clip → right-click → Copy Effects → select destination clips (Shift-click for a range, Ctrl-click for individual ones) → right-click → Paste Effects. Export: 1080p, 30fps, H.264, mp4.
6. **Thumbnail** — separate striking image prompt (character mid-ground, dramatic pose, negative space in upper third for text), finished in Canva with bold title text overlay. Not the same as the CapCut project cover.
7. **Upload (YouTube Studio)**:
   - Title format: "POV: You're the Fixer Called In When [X]"
   - Description template: hook (2-3 sentences) + one-line premise + AI-assisted disclosure line + subscribe CTA + hashtags.
   - Tags: pov story, dark business story, corporate thriller, fixer story, [industry-specific terms], ai story, short story youtube. **Tags go in the dedicated Tags field** (click "Show more" further down the upload panel), not pasted as text into the description — the description doesn't function as tag metadata even though it looks similar.
   - Toggle **Altered or synthetic content: Yes** under "Show more" during upload (no penalty to reach/monetization, protects against forced auto-labeling).
   - Audience: Not made for kids. Category: Entertainment.
   - Add to the **"POV: The Fixer"** playlist (sort order: oldest first, so new viewers hit video 1 first).
8. **Short (per published episode)** — rebuild, don't clip. Cropping 16:9 Ken Burns-panned images to 9:16 throws away the composition; instead generate 3–4 new vertical images matching the style anchor for a 45–60 second self-contained beat (tension → turn → payoff). Add captions on Shorts specifically (long-form stays caption-free by design, but Shorts are watched sound-off-then-on in scroll). Only ever source a Short from an **already-published** episode — never an upcoming one, since it spoils the thing it's meant to drive traffic toward. Target cadence: one Short per published long-form episode. ElevenLabs cost is negligible (~800 characters, under 1% of monthly quota) — the real cost is production time, not credits.

## Channel setup (already done)

- Phone verified (needed for >15 min uploads).
- Playlist "POV: The Fixer" created — public, oldest-first sort.
- Posting schedule starting point: Tue/Thu ~2pm ET, Sat ~12pm ET — placeholder until real Studio analytics ("When your viewers are on YouTube") replace it.
- Decided against paid video promotion for early videos — promoted views don't count toward the 4,000 watch-hour monetization threshold, and there's no track record yet to justify spending on amplification.

## Video 1 — COMPLETE

**Title**: POV: You're the Fixer Called In When a Factory Worker Dies
**Company**: Meridian Industries (industrial manufacturing)
**Plot beats**: 2am call about a factory death → drives to plant → plant manager hands over logbook under pressure → discovers ignored maintenance request → finds the safety guard deliberately removed to protect quota numbers → confronts floor supervisor, traces pressure up the chain → finds corporate letterhead document treating safety compliance as a cost to minimize → drives to corporate HQ tower → elevator up → confronts the founder (silver-haired, standing at the window) and his son (who tries to buy the problem away) → Fixer sets his own terms instead: real payout to the dead worker's family, genuine safety audit, quiet removal of the executive who set the quotas, keeps the folder as leverage → walks out at dawn → phone buzzes with a new, unnamed incoming call (hook for video 2).
**Status**: Published, live on the channel.

## Video 2 — COMPLETE
## Video 2 — COMPLETE

**Title**: POV: You're the Fixer Called In When a Whistleblower Disappears
**Company**: Ashford Reyne (private bank / family office)
**Plot beats**: 2am call from general counsel Gareth Thorne — compliance officer Nora Vance has gone dark after escalating suspicious transactions tied to a shell company (Kestrel Holdings) → arrives at the bank, meets Thorne and principal Vivian Ashford → searches Nora's office, finds signs of a rushed departure rather than a planned one → stairwell conversation with coworker Priya Raghavan reveals a man visited Sunday, before Nora's official disappearance timeline → finds Nora's car abandoned in the garage since Sunday, proving Thorne's timeline omitted a day and a half → apartment has been professionally searched → traces Nora via a photo of her late aunt to a laundromat apartment → finds her hiding, armed, distrustful → she explains the layering structure moving money just under reporting thresholds, and that she was hunted after finding it → a tail confirms someone followed them, tests the door, leaves → confronts Thorne, who breaks and reveals Vivian ordered the search → confronts Vivian at dawn — she offers a bribe, Fixer refuses and dictates terms instead (Kestrel relationship terminated and reported, Nora reinstated with back pay and no retaliation, Fixer keeps the folder) → walks out, drops Nora off, phone rings with the next unknown call.
**Status**: Published — https://youtu.be/UqmaIN8lLvE
**Playlist**: POV: The Fixer
**Image count**: 26 originally planned, grew to 32 (+ title card) during production as scenes were broken into individual beats rather than one image per scene — see Lessons learned below.

---

## New Chat Starter Prompt

*(Copy everything below into a new Claude conversation to resume production)*

```
We're producing the next episode of "The Fixer Files," a POV YouTube series. I've got a production bible in Obsidian at "Fixer Files/Production Bible.md" — please read it first to get full context on the character, visual style, pipeline, and what's already been made (Video 1 is done and published).

For this next video, help me:
1. Land on a new crisis/industry premise (different from Video 1's manufacturing setting)
2. Write the full narration script (~2,800-3,200 words, second person "you," clean prose ready to paste into ElevenLabs)
3. Break it into ~20-25 image prompts using the established visual style anchor and character reference method
4. Give me a thumbnail prompt once we lock the premise

Let's start with the premise.
```
# Lessons learned — platform & business realities
Notes that don't fit neatly into the per-video pipeline steps, but matter for decision-making.

**Self-views don't help and can quietly hurt.** Watching your own upload from multiple personal accounts doesn't count toward monetization watch hours — YouTube filters this — and tells you nothing about real performance. Heavy self-viewing patterns can occasionally trigger a spam-view flag. Don't judge a video's performance this way; wait for real outside traffic before drawing conclusions.

**Monetization math, for calibration.** Full YouTube Partner Program (ad revenue) requires 1,000 subscribers + 4,000 valid public watch hours in the trailing 12 months, or 10M Shorts views in 90 days as an alternate path. A lower "expanded access" tier (500 subscribers + 3 uploads in 90 days + 3,000 watch hours or 3M Shorts views) unlocks fan funding only (Super Thanks, memberships), not ad revenue. A channel's first video or two won't produce meaningful signal either way in views, watch hours, or retention — don't over-read early numbers.

**Shorts vs. long-form isn't either/or.** Long-form is what actually accrues toward the 4,000-hour threshold and carries ad revenue; Shorts views don't count toward that clock. But Shorts are the only real discovery mechanism for a channel with no subscriber base yet, and they're nearly free on the ElevenLabs budget (see pipeline step 8). Keep long-form as the core deliverable; treat Shorts as a byproduct of already-published episodes, not a parallel content operation competing for the same hours as scripts.

**CapCut has no true "select all clips" — use range-select or Copy/Paste Effects instead.** Shift-click for a contiguous range, Ctrl-click to add individual clips, then Copy Effects / Paste Effects (right-click menu) to push animations, filters, or adjustments across the selection in one action. Transitions specifically have their own "Apply to All" button in the transitions panel.