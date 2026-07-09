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

1. **Premise + outline** — pick a new crisis/industry, sketch cold open → discovery → confrontation → resolution → new-call hook.
2. **Script** — full narration written in second person, ~2,800–3,200 words for an 18–20 min runtime at ElevenLabs' natural pace. Delivered as clean prose, no scene labels, ready to paste directly.
3. **Scene/image prompts** — roughly 20–25 unique images per video (images held ~15–20s each with pan/zoom in CapCut, not cut every 5–10s). Each prompt built from the visual style anchor + character reference line + scene-specific description.
4. **Voice (ElevenLabs)** — Creator plan, Simeon voice ("Reassuring, Deep, and Smooth"), Eleven Multilingual v2 model. Use **Studio** (not the basic Text to Speech tool) to avoid the 5,000-character truncation limit. Export as single MP3 file.
5. **Assembly (CapCut)** — audio track first, images laid in over it in story order, pan/zoom (Ken Burns) animation applied per image, no captions (deliberate style choice), no music yet (may revisit for future videos). Export: 1080p, 30fps, H.264, mp4.
6. **Thumbnail** — separate striking image prompt (character mid-ground, dramatic pose, negative space in upper third for text), finished in Canva with bold title text overlay. Not the same as the CapCut project cover.
7. **Upload (YouTube Studio)**:
   - Title format: "POV: You're the Fixer Called In When [X]"
   - Description template: hook (2-3 sentences) + one-line premise + AI-assisted disclosure line + subscribe CTA + hashtags.
   - Tags: pov story, dark business story, corporate thriller, fixer story, [industry-specific terms], ai story, short story youtube.
   - Toggle **Altered or synthetic content: Yes** under "Show more" during upload (no penalty to reach/monetization, protects against forced auto-labeling).
   - Audience: Not made for kids. Category: Entertainment.
   - Add to the **"POV: The Fixer"** playlist (sort order: oldest first, so new viewers hit video 1 first).

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
