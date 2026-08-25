# Metropolis — Live Cue

A full-screen cue display for playing live to the 148-minute *Complete Metropolis*
(the 2010 restoration). Built for one iPad on a music stand, with a second pair of
eyes on it from the drum riser.

It answers three questions, continuously and from three metres away:
**where are we in the film**, **what is about to happen**, and **when is the next
downbeat**.

Open `index.html` — that is the whole app. No server, no build, no dependencies,
no network. Everything lives in the browser's local storage on that one device.

---

## Getting it onto the iPad

1. Open the page in Safari.
2. **Share → Add to Home Screen.**
3. Launch it from the home-screen icon.

That gives you a real full-screen app with no browser chrome, and a service
worker keeps the whole thing cached — it opens with the venue wifi off, or with
no wifi at all. (In a normal browser tab the ⛶ button in the top right does
full screen instead.)

---

## The two clocks

**Free run** — the film comes from a projector or another machine, and the app
keeps its own clock. This is the default. You start it by hand and keep it
honest with the sync points.

**Film clock** — you load your own copy of the film in **SETUP → VIDEO FILE**.
The video becomes the clock, so there is nothing to calibrate: scrubbing the
timeline scrubs the picture, pre-rolls seek, and the two can't drift apart. The
file is read straight off the iPad and never leaves it. iPadOS drops the handle
when the app restarts, so re-pick it each session.

The film window sits **DOCK**ed in the left column by default, or **FLOAT**s
where you drag it, or goes **FULL** screen behind the cue overlay.

---

## Syncing to the film

Cues ticked as **sync points** are moments with an unmistakable frame — the
Moloch vision, the transformation, the stake. They are the calibration handles.

1. Tap **SYNC**. The list opens with the nearest landmarks first.
2. Wait, watching the screen.
3. Tap the landmark **at the instant it lands**.

The clock jumps to that timecode, less your tap-latency allowance
(**SETUP → TAP LATENCY**, 150 ms is a fair start). If it was stopped, it starts.

For smaller corrections the transport has **−1s / −.1 / +.1 / +1s**, live, at any
time.

### Drift, and how to kill it

Over 148 minutes a print running at 24 fps against a clock expecting 23.976 goes
about nine seconds adrift. Nudging that away all night is miserable, so the app
measures it instead.

Sync **twice, at least two minutes apart**. The second tap works out how fast
the app's clock is running against the print and reports the drift per hour in
the confirmation. **SETUP → CLOCK RATE → USE MEASURED** then applies the
correction for the rest of the show, and a `RATE` chip appears in the status bar
so you know it is on. Free run only — the film clock cannot drift from itself.

---

## The cue sheet

**CUES** opens it. Every cue has:

| Field | What it does |
|---|---|
| **Title** | The big text in NOW and NEXT |
| **Timecode** | Film time. `1:02:33.5`, `62:33` and `3753` all parse |
| **Kind** | `SONG` (also builds the running order) · `CUE` · `SCENE` · `ACT` |
| **Heads-up** | What is about to happen and what you do about it |
| **Lead time** | How early it takes over the NEXT panel. Blank = the global default |
| **Tempo / beats per bar / count-in bars** | Turns the cue into a count-in |
| **Sync point** | Puts it in the SYNC list |

### Count-ins

Give a cue a tempo and it earns a count-in. The specified number of bars before
the downbeat, the whole screen goes over to a giant beat number, a dot per beat,
and the bars remaining. Once the song is under way a `BAR n · beat` readout keeps
running in the NOW panel for as long as that cue holds.

**CLICK** in the transport cycles **OFF → COUNT-IN → THROUGH**. Count-in mode
clicks only the bars before the downbeat; through mode keeps going for the
length of the cue. Level is in SETUP. It is a rehearsal guide, on the iPad
speaker or whatever it is plugged into — not a click track worth putting in
anyone's ears at a show.

### Building the real sheet

The seed sheet ships with the film's landmarks **in the right running order**
but with **estimated timecodes**, flagged with an amber bar and counted in the
`n EST` chip. They are a scaffold, not data — the ordering is what saves you
typing, the numbers are what you replace.

To build the real thing, in one pass:

1. Load the film (or start it on the projector and free-run alongside).
2. Open **CUES** and leave it open.
3. As each moment lands, hit **STAMP** on its row. That writes the live playhead
   into the row and clears the estimate flag.
4. **STAMP NEXT** in the header does the same to whichever cue is next, so you
   can work down the list without hunting for rows.

**+ ADD** drops a new cue at the current playhead. **EXPORT** writes the sheet
and settings to a JSON file — do that before every show, and keep it somewhere
that is not the iPad. **IMPORT** reads it back, on any device.

Delete the two `EXAMPLE —` cues once you have seen how a count-in behaves.

---

## Rehearsing from anywhere

- **GO TO…** — type a timecode, or pick any cue. **Pre-roll** (on by default)
  starts you far enough ahead that the approach and the count-in play out
  properly, rather than dumping you on the downbeat.
- **⏮ CUE / CUE ⏭** — previous and next cue, both with pre-roll.
- **The timeline** — tap or drag anywhere to scrub. Act boundaries are marked;
  songs are the tall gold ticks.
- **The running order** in the left column lists your `SONG` cues, with the
  current one green and the next one gold. Tap any of them to go there.

---

## Keyboard (for setting it up on a laptop)

| Key | |
|---|---|
| `space` | start / pause |
| `←` `→` | nudge ∓1s (`shift` for 10s) |
| `↑` `↓` | nudge ±0.1s |
| `s` | sync list |
| `g` | go to |
| `e` | cue sheet |
| `n` / `p` | next / previous cue |
| `c` | cycle the click |
| `f` | full screen |
| `esc` | close whatever is open |

---

## Files

    index.html            the entire app
    sw.js                 offline cache — bump CACHE to ship an update
    manifest.webmanifest  home-screen install
    icon.svg
