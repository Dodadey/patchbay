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

### Which clock, and when the window is worth having

**SETUP → CLOCK SOURCE** decides what the loaded file is *for*. Get this right
and the window is worth having; get it wrong and the app confidently follows
the wrong film.

**FILE IS THE CLOCK** — when this iPad's copy is the only film in the room.
Rehearsing on the sofa, or working through the cue sheet. Nothing can drift,
scrubbing seeks the picture, pre-rolls land on the frame. This is where the
window earns its keep, and where you should build the cue sheet.

**FILE FOLLOWS** — when the audience is watching a projector or another
machine. That is a second, independent player with its own clock, and the one
on the wall is the one that matters, so the app free-runs, you hold it to the
projected print with the sync points, and this copy gets dragged along behind:
the app matches its speed and re-seeks it whenever it wanders more than a third
of a second. Syncing or nudging the clock moves the picture with it.

In that second case the window is a **sync check**, not a source of truth — a
glance to confirm the app and the wall still agree. That is genuinely useful,
but it is not free: a 148-minute decode is heat and battery. Keep it DOCKed and
small, or leave it off and trust the sync points, which is what they are for.

If the film is coming *off the iPad itself* into a projector, don't use this
window at all — mirroring puts the whole cue display on the wall. Play the film
from a separate machine and let the iPad be the cue display.

### What format the film should be in

This is Safari's decoder, so the safe answer is narrow:

| | |
|---|---|
| Container | **`.mp4`** (or `.m4v`). **`.mkv` will not open at all** |
| Video | **H.264 / AVC**. HEVC also decodes in hardware on a modern iPad and halves the size |
| Avoid | **VP9 and AV1** — no hardware decode, so 148 minutes of stutter and heat |
| Size | The film is 4:3. **960×720** is plenty for a docked window; 1280×960 if you want FULL to look good. 1080p is wasted on a reference picture |
| Bitrate | ~2 Mbps at 720p — about 2 GB for the whole film |
| Audio | **Strip it.** It is a silent film and you do not want the restoration's score leaking out of the iPad |
| Frame rate | **Leave it alone**, and make it constant. Do not let a converter "smooth" it to 30 |

In HandBrake: a **Fast 720p** preset, framerate set to *Same as source* and
*Constant Framerate*, and the audio track removed. Or, in one line:

    ffmpeg -i source.mkv -an -c:v libx264 -crf 22 -preset slow       -vf scale=-2:720 -movflags +faststart metropolis-720.mp4

**Check the runtime before you build the cue sheet.** If your file comes in
around **142 minutes** rather than 148, it is a PAL-speed transfer running 4%
fast, and every timecode you stamp against it will be wrong against a 24 fps
projection. Either work from a 24 fps copy, or accept that this file is
rehearsal-only and build the sheet against the print you will actually play to.

### Where to keep the file

In the **Files** app, under **On My iPad**. The picker in SETUP reads from
there.

If it lives in iCloud Drive and has not been downloaded, picking it will stall
while iOS fetches it — long-press the file and choose **Keep Downloaded** first.
Leave a few GB of free space.

Nothing is uploaded and nothing is copied: the app reads the file where it sits,
and it never leaves the iPad. The flip side is that iPadOS drops the handle when
the app restarts, so **re-pick the file each session**. It takes two taps and
the app remembers the name to remind you.

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
| **Tempo / time signature / count-in bars** | Turns the cue into a count-in |
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

### Entering the whole set at once

**CUES → SONG SHEET** is the fast way in. One song per line:

    time , title , tempo , beats-per-bar , count-in bars , note

    1:02:33 , Moloch , 96 , 4/4 , 2 , comes in on the explosion
    0:38:00 , Babel , 72 , 3/4 , 1
    ? , Yoshiwara , 118 , 6/8 , 2 , dance sequence

Only the **title** is compulsory. No tempo means no count-in; no beats-per-bar
assumes 4; no count-in bars assumes 4. Tempos read the same written `96`,
`96bpm` or `♩=96`. Signatures can be `6/8`, `3/4` or just `6` — the top number
is what gets counted. Lines starting with `#` are ignored.

Separators are whatever the line happens to use — tabs, commas, vertical bars,
or runs of spaces — so a column pasted out of Numbers or Excel lands the same
way as something typed on the iPad. The preview underneath shows what the app
made of every line, with anything it could not read called out in red, before
you commit.

**Don't know the timecodes yet?** Put `?` in the time column or leave it out.
Those songs keep the order you typed them, get spread across the film, and are
flagged as estimates — then pin them down with a play-through as below.

**REPLACE ALL SONGS** swaps the whole set for what is in the box; **ADD TO
SHEET** appends. Neither touches scene or act markers. **LOAD CURRENT** fills
the box with the songs already in the sheet in this same format, so you can
edit the lot as text and put it back.

### When your source has a lead-in

Almost no copy of the film starts on the first frame of picture. Streams and
transfers open with title cards, a restoration logo or a black head, and often
carry extra material at the end.

None of that matters to the app, which counts an abstract **film time** rather
than anything about your file. What matters is that you are **consistent**:
build the cue sheet against the same source you will play to, and read
timecodes the same way every time.

There are two ways to be consistent, and either works:

- **Zero at the source's zero.** Timecodes match whatever the player's readout
  says. Simplest if you always play from that one source; the lead-in and the
  tail are just dead air with no cues in them.
- **Zero at the first frame of picture.** Portable to any print, but you have
  to subtract the lead-in from every reading you take.

If you get it wrong, or change source, **SHIFT** in the cue sheet fixes it in
one move. Stamp the one cue you can actually see, work out how far it moved,
type the difference and press **ALL CUES**. Type `+47` for forty-seven seconds
or `−1:12` for a minute twelve; the opposite sign puts it back. **FROM PLAYHEAD
ON** shifts only what comes after the current position, for a print that gains
an extra title card partway through. Cues pushed below 0:00 pile up at zero and
the app says so — that part cannot be undone by shifting back.

### Working alongside an external player

If the film comes from Apple TV, a projectionist's machine or anything else you
cannot load as a file, the app free-runs and the sync points hold it to the
picture. The one nuisance is that your player's scrubber and the app's film
time will not read the same number, because the player counts from the start of
its own file, lead-in and all.

#### Putting the film beside it on one iPad

A DRM-protected stream — Apple TV, or any other bought film — cannot be pulled
into the page. There is no embeddable URL, iPadOS Safari has no screen-capture
API, and protected video captures as black frames anyway. iPadOS will do it for
you at the system level instead:

- **Picture in Picture** — start the film in the TV app, put it in PiP, then
  switch to the cue app. The film floats over the top, movable and resizable.
  Best for rehearsing, when you want the cue display big.
- **Split View** — the TV app one side, this the other. Best for building the
  cue sheet, where you need room for the cue list and its STAMP buttons.

The layout is built for this: at Split View and Slide Over widths it collapses
to a single column with the clock, running order and NEXT panel all still
readable, and the transport bar scrolls sideways rather than truncating.

Either way the app cannot see where the film has got to, so this is still free
run plus sync points. Which is what the offset below is for.

**SETUP → PLAYER OFFSET** settles that. Press **WORK IT OUT**, type whatever
your player is showing at that moment, and the app works out the difference. A
**PLAYER** readout then appears next to the film clock, so you can call out
either number without doing arithmetic, and a **PLAYER TIME** box appears in
**GO TO** so a position quoted off someone else's scrubber can be typed
straight in.

The offset is display only. Cue timings, sync points and count-ins all stay in
film time and are untouched by it.

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
