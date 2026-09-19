# APBA Football — Scorekeeper

Score, chains and a full box score for the **APBA Pro Football** board game,
built for one iPad sitting next to the board.

It answers the three things you otherwise keep on a paper sheet and keep
getting wrong: **what is the down and distance**, **where is the ball**, and
**what did everybody actually do**.

Open `index.html` — that is the whole app. No server, no build, no
dependencies, no network. Everything lives in the browser's local storage on
that one device.

---

## Getting it onto the iPad

1. Open the page in Safari.
2. **Share → Add to Home Screen.**
3. Launch it from the home-screen icon.

That gives you a real full-screen app with no browser chrome, and a service
worker keeps the whole thing cached — it opens with the wifi off, or with no
wifi at all.

It is built for landscape on an iPad, but it folds down to one column on a
phone or in Split View, and works fine in a laptop browser for setting leagues
up.

---

## It keeps a play list, not a scoreboard

The only thing stored is the list of plays. Score, downs, field position,
drives, time of possession and every individual statistic are worked out from
that list afresh on each render.

That one decision is what makes the rest trustworthy:

- **Undo** takes the last play off and everything follows it back — including
  the box score.
- **Tap any line in the play-by-play** to correct or delete it. Fix a 7-yard
  run that was really a 17-yard run in the second quarter and the fourth-quarter
  field position, the drive chart and the rushing totals all move with it.
- The scoreboard can never quietly disagree with the box score, because
  neither is stored.

If a correction strands a later play — you delete a kickoff, so the run that
followed it has nobody holding the ball — that play is marked **out of
sequence** in red and skipped, rather than being guessed at. Correct or delete
it and the sheet closes up.

---

## Recording a play

The right-hand pad only ever offers what is legal: kickoffs when a kickoff is
due, the try after a touchdown, and run/pass/sack/punt/field goal the rest of
the time. Penalty, kneel, spike, timeout and note are always to hand.

Pick the play, tap the players, set the yardage on the big stepper, and record
it. The button tells you what the board will say before you commit:

    Record play        → 2nd & 4 at GB 38

Everything that follows from the play is worked out for you: first downs,
turnovers on downs, touchdowns when the ball crosses the goal line, safeties
when it goes back over the other one, the try, the kickoff after a score, the
free kick after a safety, possession after a punt, and where a missed field
goal comes back to.

### The yardage

The stepper carries −10 / −5 / −1 / +1 / +5 / +10 and the number itself is
typeable. Negative numbers are red. The play never needs a yard *line* typed
in: gains are relative, and the app knows where the ball was.

The exceptions are the ones where the board really does hand you a spot rather
than a gain — where a kickoff was caught and returned to, where an onside kick
or a blocked punt was recovered — and those ask for the yard line in the plain
way a broadcaster would say it: *their own 24*.

### Fumbles, interceptions and returns

Toggle **Fumble LOST** or **Intercepted** and the fields you need appear: who
recovered it, how far it came back. A return that reaches the end zone scores
six for the defence and puts the try in front of the right team. A fumble in
your own end zone is a safety.

**Fumbled** on its own — recovered by your own side — counts in the team
fumble column without changing possession.

### Penalties

Offence or defence, 5/10/15 or anything you type, with **automatic first down**
and **loss of down** as toggles. Half-the-distance is applied for you at both
ends of the field. A penalty is not a down: an offensive foul repeats it and
lengthens the chains, a defensive one either moves the sticks or does not.

---

## The APBA line

The strip along the bottom of the pad is optional and exists for leagues that
want the roll behind every play on the record.

- **The dice**, read the APBA way: the white die first, the red second, so a
  white 4 and a red 2 is 42. Tap either die to cycle it, or **Roll** to let the
  iPad do it.
- **PRN** — the play result number off the player's card.
- **Def** — the defence that was called.

None of it affects a single number the app works out; it is carried on the play
and goes out with the export, so a disputed play can be walked back to the
card it came off. Leave the boxes empty and nothing is lost.

The app deliberately does **not** look anything up. It has no cards, no result
charts and no opinion about what should have happened — the board decides that,
and this records it.

---

## The clock

APBA editions differ, so **Menu → Teams, rosters and rules** lets you pick:

- **A running clock.** Each kind of play takes a default number of seconds off
  it — 35 for a run or a completion, 6 for an incompletion, 5 out of bounds,
  and so on. The whole table is editable, and any single play can override it
  from the **Clock** box in the pad.
- **A count of plays.** Each quarter gets a fixed number of plays and the app
  counts them down instead.

Either way the quarter goes red when it is spent and the pad offers **End
quarter**. Nothing ends a period behind your back. Halftime restores the
timeouts and gives the kickoff to whoever did not receive to open the game.

Overtime is there: end the fourth quarter and the app goes to a fifth period at
the overtime length.

---

## House rules worth checking before kickoff

Under the same setup screen, because leagues replaying different eras want
different answers:

| | |
|---|---|
| **Missed field goal** | comes back to the spot of the kick (modern), to the 20, or to the previous line of scrimmage (pre-1974) |
| **Kickoff touchback** | ball on the 25, or the 20 for an older season |
| **Kickoff from** | the kicking team's own 35, 30, or wherever your edition says |
| **Quarter length** | minutes, or a count of plays |
| **Timeouts** | per half |

---

## The teams

Tap either side of the scoreboard to open that club's roster.

Every club starts with a skeleton of numbered slots — `#12 QB`, `#22 RB`,
`#3 K` and so on — so you can record a play the moment you open the app and
name people later. Only the players you actually record plays for need names.

**Paste a list** takes a whole roster at once, one player per line:

    12 , QB , Fran Tarkenton
    22 , RB , Chuck Foreman
    80 , WR , Sammy White
    3  , K  , Fred Cox

Separators are whatever the line happens to use — commas, tabs, vertical bars,
or runs of spaces — so a column pasted out of Numbers or a text message lands
the same way as something typed on the iPad. Number and position can both be
left out. Lines starting with `#` are ignored.

The position matters for one thing only: which players the pad offers first.
Ball carriers get RB/FB/QB/WR/TE, passers get QB, kickers get K. **+ all**
opens the whole roster when the halfback throws.

---

## What it keeps

**Box score** (also the `b` key) gives you the lot:

- line score by quarter, overtime included
- scoring summary, with the running score after each one
- team statistics — first downs by type, net yards, yards per play, third and
  fourth down efficiency, sacks, punting average and punts inside the 20,
  penalties, turnovers, red zone, time of possession
- passing, rushing, receiving, kicking, punting, returns and defence for every
  player, with NFL passer rating
- the full play-by-play

**Drive chart** lists every drive: where it started, how many plays, how many
yards, how long it took and how it ended.

**Season totals** pools every game kept on the iPad into standings and league
leaders — passing, rushing, receiving, kicking, punting and defence — matched
up by club abbreviation and shirt number, so a league season builds itself as
you play it.

---

## Getting it back out

- **Copy as text** puts a plain-text box score on the clipboard, ready to paste
  into a league email or a forum post.
- **Export game** writes the whole game — teams, rules, every play with its
  dice and result numbers — to a JSON file.
- **Import** reads one back, on any device.

Keep the exports somewhere that is not the iPad. Local storage is local
storage: clearing Safari's site data takes the season with it.

**Games on this iPad** lists everything kept, opens any of them, and starts the
next game with the same two clubs and the same house rules already set.

---

## Keyboard (for setting a league up on a laptop)

| Key | |
|---|---|
| `enter` | record the play |
| `u` | undo |
| `b` | box score |
| `r` | roll the dice |
| `esc` | close whatever is open |

---

## Two things it does not do

**It does not play the game.** No cards, no charts, no dice tables. The board
and your opponent decide what happened; this writes it down and adds it up.

**Two-point conversions are scored but kept out of the rushing and receiving
totals**, the way the league books have always done it.

---

## Files

    index.html            the entire app
    sw.js                 offline cache — bump CACHE to ship an update
    manifest.webmanifest  home-screen install
    icon.svg
