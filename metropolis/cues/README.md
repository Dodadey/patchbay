# Cue sheets

Importable sheets for the app. **CUES → IMPORT** takes one of these files.

Import **replaces the whole sheet**, so export what you have first if it
contains stamped timecodes you care about.

---

## `huppertz-scene-cues.json`

263 cues covering roughly the first two thirds of the film.

Huppertz wrote his 1927 score to picture, annotating it shot by shot, so the
score doubles as a scene-by-scene map of the film. This sheet is that map:
his annotations, in his order, with an English reading of each and the German
original kept underneath so any translation can be checked. It carries no
musical information at all — no tempo marks, no score references, nothing about
the music, which is not being used.

| | |
|---|---|
| Scene cues | 257 |
| Songs | 4 — Underworld, Games, Maria, Machines |
| Act markers | 2 — Auftakt, Zwischenspiel |
| Sync points | 137 |
| Flagged as estimates | 259 |

### What to trust

**The order is reliable.** It is the composer's, from someone who worked
through the film frame by frame.

**The times are not.** Only the four songs carry verified timecodes. Every
scene cue is a placeholder, flagged as an estimate and shown with an amber bar
in the cue sheet. They were spread between the four verified anchors by
position, which assumes every cue lasts the same length — they do not, so the
early rows in particular read as obviously wrong. Replace them with **STAMP**.

### What is missing

The **Furioso** — the whole final act. The scan's text layer ran out around
page 78 of 161, and re-splitting the PDF produced image-only files with nothing
to read. Build that act with **MARK** instead: playing or scrubbing through and
marking as you go produces real timecodes rather than placeholders, which is
better data than this file holds anyway.

Expect a few cues for footage that is not in the 148-minute restoration.
Huppertz scored the 1927 premiere version, which was longer, and some of it is
still lost. Those rows want deleting, not stretching.

---

## Keeping your own work

The app stores everything in one browser's local storage on one device. It is
not backed up anywhere and does not travel between devices.

**CUES → EXPORT** after every stamping session, and keep the file somewhere
else. The app can be rebuilt from this repository; timecodes established
against an actual print cannot.
