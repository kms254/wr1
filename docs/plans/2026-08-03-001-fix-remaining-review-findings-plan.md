---
title: Remaining WR1 Review Findings - Plan
type: fix
date: 2026-08-03
artifact_contract: ce-unified-plan/v1
artifact_readiness: implementation-ready
product_contract_source: ce-plan-bootstrap
execution: code
---

# Remaining WR1 Review Findings - Plan

**Target repo:** wr1

## Goal Capsule

- **Objective:** Close the two review findings from the 7-lens WR1 site audit that weren't already fixed live in the review conversation.
- **Authority hierarchy:** This plan > the review conversation's findings > general site conventions already established in `shopping-list/build/index.html` and `BRANDING.md`.
- **Stop conditions:** Stop and ask only if no defensible figure at all can be established for U1 — including KTD1's general-literature fallback — or if no real, verifiable video can be found for one of the three topics in U2. KTD1's fallback range is the resolution path for "CURV LAB's exact figure isn't accessible," not a stop condition.
- **Execution profile:** Small, two independent units, no shared dependency. Either can ship alone.
- **Tail ownership:** Whoever executes verifies tag balance and JS syntax on both `shopping-list/build/index.html` and `shopping-list/index.html` (established pattern in this repo — see recent commit history) before committing.

## Product Contract

### Summary

Correct the build guide's heat-set insert temperature claim, which understates the real PETG range, and add three "further learning" videos to Session 15 (multimeter basics, battery polarity, soldering basics) to support the hands-on soldering step that's staying as currently framed.

### Problem Frame

A 7-lens review of the WR1 companion site (educational content, instructional clarity, adversarial fact-check, logical consistency, grammar, physical safety, code correctness) ran across all five pages and surfaced roughly 60 findings. Nearly all of them — every CRITICAL and HIGH item, and most MEDIUM items — were fixed and shipped in four commits during the review conversation itself. Two items were deliberately held back for the site owner's decision rather than silently fixed:

1. The educational-content review flagged Session 1's stated heat-set insert temperature (250-350°F) as likely too low for PETG, recommending verification against CURV LAB's source guide rather than guessing a correction.
2. The safety review flagged two "age-fit" judgment calls — Session 15's high-current solder joint, and Sessions 9-11's e-clip pliers hand-strength — as decisions for the site owner, not text to change unilaterally.

Both judgment calls have now been resolved by the site owner: keep Session 15's current solder framing (kid solders with an adult guiding her hands; adult alone does the final polarity check) and add supporting videos instead of changing who does the physical task; leave the e-clip hand-strength call as something judged in the moment at the bench, with no text change. This plan covers what's left: the temperature correction, and the three videos the solder-posture decision calls for.

### Requirements

- R1. The site's stated heat-set insert temperature reflects a real, defensible figure for PETG-HF, consistent across every place it appears.
- R2. Session 15's connector-conversion step links three real, verified videos — multimeter basics, battery/LiPo polarity, and soldering basics — following the same `.learn-more` pattern and verification standard already used throughout the build guide (real video confirmed via YouTube's oEmbed endpoint before its title/channel is written into the page; no fabricated or guessed titles).

### Scope Boundaries

- Already resolved in the review conversation and out of scope here: all CRITICAL/HIGH findings across all seven review lenses, plus the MEDIUM/LOW findings that were fixed (safety framing, video misattributions, cost-calculator scaling, instructional-clarity gaps, grammar, Brand Guide CSS drift, dead CSS, doc drift). Re-touching any of that is not part of this plan.
- Explicitly declined by the site owner, not part of this plan: adding adult-takeover text to the e-clip steps (Sessions 9-11) — left as an in-the-moment judgment call at the bench.
- Explicitly declined by the site owner, not part of this plan: moving Session 15's solder joint to fully adult-solo — the current kid-solders/adult-guides framing stays as-is.
- Deferred, not part of this plan: a liveness sweep of the site's ~65 external product/video links (flagged as LOW/INFO by the code-correctness review as worth doing separately, not urgent).

## Planning Contract

### Key Technical Decisions

- **KTD1 — Source the temperature correction from general PETG heat-set literature if the exact CURV LAB guide figure can't be confirmed, but check the PETG-HF distinction first.** Cross-referencing during planning found the real PETG heat-set insert range is approximately 464-473°F (240-245°C) — well above the site's current 250-350°F, which is too cold to properly seat a PETG insert. The corroborating sources (Printables, CNC Kitchen, FacFox) describe generic PETG, not the HF variant this build actually uses — before falling back to that range, check whether PETG-HF has its own published heat-set figure (CURV LAB's guide, or the filament manufacturer's own docs) that differs from generic PETG; prefer a PETG-HF-specific figure if one exists. Lead with °F to match the site's existing Fahrenheit-only convention (no page currently uses °C), with °C available parenthetically for precision.
- **KTD2 — Verify videos the same way the review conversation did, and confirm actual content, not just metadata.** Two of the site's existing videos were found during the review to have real content that didn't match their displayed title or attribution (one had a fabricated-sounding title that didn't match the actual video; one was credited to the wrong channel). Both were caught by checking `https://www.youtube.com/oembed?url=<video-url>&format=json` and comparing the real `title`/`author_name` against what was about to be written on the page. Apply the same oEmbed check to all three new videos before writing their `lm-title`/`lm-hook` — but the oEmbed check only confirms the title and channel are real, not that the video actually teaches the claimed topic. Watch enough of each video to confirm its content matches what its `lm-hook` will claim before writing it.
- **KTD3 — Place the three videos in Session 15's existing connector-conversion block, not a new section.** The videos support content already on the page (the multimeter polarity check, the "+/-" battery warning, and the hands-on solder steps), so they extend the existing `.learn-more` pattern in place rather than introducing new page structure.

## Implementation Units

### U1. Correct the heat-set insert temperature claim

- **Goal:** Replace the site's inaccurate 250-350°F insert temperature with a verified, accurate figure for PETG-HF, consistent everywhere it's stated.
- **Requirements:** R1
- **Dependencies:** none
- **Files:**
  - `shopping-list/build/index.html` (Session 1's "Why we use a press instead of a handheld iron" swap-note, AND Step 2's body text — both currently state "250–350°F" and both need the same correction)
  - `shopping-list/index.html` (the "Threaded-Insert Soldering Tips" tool-card note)
- **Approach:** Check all three locations for the current wording, attempt to confirm the exact figure against CURV LAB's actual guide PDF (linked from the build guide's credit box) if accessible, checking specifically whether it distinguishes PETG-HF from generic PETG; otherwise use 464-473°F (240-245°C) per KTD1. Update all three locations to match — they currently all state the same wrong number and should continue to agree after the fix. As a final check, grep the whole site for the old "250" / "350°F" string to confirm no other occurrence was missed.
- **Test scenarios:** Test expectation: none -- pure factual/content correction, no interactive behavior change.
- **Verification:** All three locations state the same, sourced temperature figure, in the site's existing °F-first convention. If the exact CURV LAB figure couldn't be confirmed, the plan's fallback range (464-473°F / 240-245°C) is used and no single-source guess is presented as certain.

### U2. Add multimeter, battery-polarity, and soldering videos to Session 15

- **Goal:** Give the kid explanatory context for the three concepts now load-bearing in Session 15's connector-conversion step, without changing who performs which physical task.
- **Requirements:** R2
- **Dependencies:** none
- **Files:** `shopping-list/build/index.html` (Session 15's "Converting the ESC connector" step block, alongside the existing XT60/EC5 diagrams)
- **Approach:** Find and verify three real videos, one per topic, each placed in exactly one step's `.step-body` — every existing `.learn-more` card in this file lives inside a single step, never spanning two steps or sitting in the swap-note, so these three follow that same one-card-per-step-body pattern:
  - Soldering basics for a first-timer — placed in Step B's step-body (the first hands-on solder step); its `lm-hook` can note the same technique carries into Step D without needing a second card there.
  - Battery/LiPo polarity — why reversed polarity is dangerous, placed in Step D's step-body (where the "+ to +, − to −" polarity match actually happens), not the swap-note above the step-list.
  - Multimeter basics — what it measures and how to read it, placed in Step E's step-body, tied to the adult's final polarity check.

  For each, confirm the real title and channel via YouTube's oEmbed endpoint before writing anything on the page (KTD2), and watch enough of the video to confirm its actual content matches the topic — do not trust a search result's displayed title, and do not rely on metadata match alone. Follow the exact `.learn-more` markup pattern already used elsewhere in this session (`lm-label`, `lm-title` as `Channel — Real Title`, `lm-hook` tying it back to the specific step it supports).
- **Test scenarios:** Test expectation: none -- content addition following an established markup pattern, no new interactive behavior.
- **Verification:** All three videos resolve via oEmbed with real titles/channels matching what's written on the page; each video was actually watched (not just metadata-checked) and its content matches the topic its `lm-hook` claims; each card sits in exactly one step's `.step-body` per the placement above; tag balance (`div`/`a`/`button`/`span` counts) and inline `<script>` syntax on `shopping-list/build/index.html` are unchanged/valid after the edit.

## Verification Contract

This is a static site with no test runner, build step, or CI. Verification is manual, matching the pattern used throughout the review conversation:

| Check | How |
|---|---|
| Tag balance | Count open vs. close tags per element (`div`, `a`, `button`, `span`) in `shopping-list/build/index.html` and `shopping-list/index.html` via `grep -o` counts; mismatches indicate a broken edit. |
| JS syntax | Extract each page's inline `<script>` block and run it through `node -e "new Function(source)"`; a thrown error indicates a syntax break. |
| Temperature consistency | Grep the whole site for the old "250" / "350°F" string after U1 lands; zero remaining matches confirms no fourth occurrence was missed beyond the three named in U1. |
| Video accuracy | Fetch `https://www.youtube.com/oembed?url=<video-url>&format=json` for each new video to confirm `title`/`author_name` are real, then watch enough of each to confirm its actual content matches what's written on the page — metadata match alone doesn't prove the content is on-topic. |
| Deploy | After pushing to `main`, poll `gh run list --repo kms254/wr1 --limit 1` until the GitHub Pages deploy workflow reports `completed success`. |

## Definition of Done

- All three locations in U1 (two in `shopping-list/build/index.html`, one in `shopping-list/index.html`) state the same corrected insert temperature, sourced per KTD1, with no other occurrence of the old figure remaining anywhere on the site.
- Session 15 has three new `.learn-more` video cards (soldering in Step B, battery polarity in Step D, multimeter in Step E), each independently verified via oEmbed and actually watched before being written.
- Tag balance and JS syntax pass on every touched file.
- Changes are committed (plain `git commit`, matching this repo's existing convention — no CE commit skill in use here) and pushed to `main`.
- The GitHub Pages deploy for `main` completes successfully.
- No abandoned exploration artifacts (scratch scripts, unused image downloads) are left in the repo.
