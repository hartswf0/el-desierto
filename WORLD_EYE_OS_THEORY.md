# WORLD / EYE SYSTEM v14.3 — theory-of-the-program

*Naurian Loop · constructed before program text · grounded in the transcription
of all 520 boot-screen states (`MONTE/os_text.json`, tesseract over 2x-negated
frames). The version number is read off the screens themselves:
`WORLD EYE SYSTEM 14.3` = 14 chapters · 3 acts.*

---

## 1. [Elicit] <purpose>

<Initial Interpretation>: the task is a <program theory problem>: **an operating
system whose chrome, vocabulary, and state machine are INFERRED from an archive
of 130 machine-boot recordings, and whose operations act on the real film
underneath.** The real-world activity it describes: an operator sits at a
terminal that boots, mounts an archive of fourteen eras, and RUNS chapters —
the OS is the projectionist's console for THE WORLD BEHIND THE EYES.

The v14.3 advance over v1 (`os.html` shipped earlier): v1 inferred <states>
from bootloader *roles* (title/prologue/act/ch/epilogue). v14.3 also infers
<operations> from bootloader *text* — every legible command on every screen is
transcribed, and the OS's verbs are the verbs the machines display. Hidden
states become visible: a screen that says `> MOVE IMAGE TO VIEW` implies a
MOVE operation and a VIEW target; a screen that says `RUN CHAPTER 03` is a
literal executable line pointing at chapter 3's film.

## 2. [Identify] <domain entities>

- <machine> — one boot recording; fields {file, ch, kind, role, static, states[4], lines[]}
- <state-frame> — one of 4 sampled frames of a <machine>; the OS's screen imagery
- <screen-line> — one OCR-transcribed line of text on a <state-frame>; evidence
- <verb> — a command word appearing on screens (RUN, MOVE, DEFINE, LOAD, EYE…)
- <program> — a chapter (1–14); owns {era clips, song, transcript, its machines}
- <drive> — an act (I/II/III); groups programs 1–5, 6–10, 11–14
- <shell> — the command line; accepts <verb> phrases, executes [operations]
- <film> — the real footage underneath: clips, cuts, the theatre program
- <operator> — the human; issues commands, watches, assumes responsibility

{system} := { <machine>×130, <program>×14, <drive>×3, <shell>, <film>, <operator> }

## 3. [Define] <operations>

Each operation is *licensed by transcription* — it exists because screens say it:

- [RUN <program|act>] — screens: `RUN CHAPTER 03`, `RUN ACT 01`. Executes a
  chapter: launch boot → chapter room, or (RUN FILM) the whole theatre program.
- [MOVE <image> TO <view>] — screen: `> MOVE IMAGE TO VIEW`. Sends a clip to
  the viewer/monitor; in the room, tap = MOVE clip TO VIEW.
- [DEFINE <symbol> = <meaning>] — screens: `DEFINE □ = CINEMA`,
  `DEFINE ○ = AUDIENCE`. The naming operation: binds a chapter's TITLE
  (the operator's monteTitles pass IS the DEFINE verb).
- [LOAD <archive>] — mounts manifests; the boot sequence's own act.
- [EYE] / [SYSTEM] — self-reference: status report (states mounted, clips,
  coverage; the sysline).
- [SEEK <t>] — the rail: jump the film to a part.
- [HALT] — shutdown; the epilogue machine plays to its static.

## 4. [Specify] <conditions>

- <served-origin> [enables] every operation (file:// blocks all fetches — known law).
- <archive-mounted> [enables] RUN/MOVE/DEFINE; before LOAD completes, only POWER.
- <machine.static> [bounds] every transition: video cuts at calibrated static.
- <program.has-clips> [enables] RUN CHAPTER; empty era [blocks] with report.

## 5. [State] <invariants>

- I1 **Evidence invariant**: every verb in the <shell> exists as a <screen-line>
  in `os_text.json`. No invented vocabulary. (The UI shows the source line.)
- I2 **Real-media invariant**: every screen image is a real <state-frame>;
  every transition is a real <machine> video; every played clip is real footage.
- I3 **Wrong-title law** (inherited): a machine announcing CHAPTER N only ever
  represents program N.
- I4 **Static law**: no machine is ever shown frozen; playback ends ≤ static.
- I5 **Return law**: Esc/BACK always walks one state up; the machine never traps.

## 6. [Map] <state transitions>

```
OFF —[POWER]→ SPLASH(title machine) —[auto]→ LOGIN(prologue machine)
  —[auto]→ DESKTOP{3 drives + X}
DESKTOP —[RUN CHAPTER n | tap program]→ LAUNCH(ch-machine n) —[static]→ ROOM(n)
ROOM(n) —[MOVE clip TO VIEW | tap]→ VIEW(clip+captions) —[close]→ ROOM(n)
ROOM(n) —[RUN]→ the chapter's film (theatre)     DESKTOP —[RUN FILM]→ theatre
any —[EYE]→ SYSTEM REPORT overlay                 any —[HALT]→ SHUTDOWN(end machine) → OFF
SHELL: visible on DESKTOP/ROOM; free text —[parse]→ one of the licensed verbs
```

## 7. [Expose] <assumptions>

- <safe> OCR noise is tolerable because lines are *evidence*, shown with their
  machine; garbled lines are filtered (≥half real tokens) not corrected.
- <safe> tesseract x2-negate reads the large command lines reliably (piloted).
- <uncertain> per-state text differences (s0 vs s3) encode a progression
  (ignition→report); v14.3 aggregates per machine, keeps per_state for later.
- <requires-user-decision> whether the shell should also accept verbs NOT on
  screens (help, ls). v14.3 answer: no — evidence invariant wins; unknown input
  answers with the nearest screen-line ("the machine only knows what it says").

## 8. [Find] <failure modes>

- OCR yields nothing for a machine → its manual page shows "UNREADABLE SCREEN·
  static only"; program still runs (role-inference fallback). 
- os_text.json missing (old deploy) → shell hides; v1 behavior remains.
- Command parse fails → shell echoes the machine's own nearest line, never a
  generic error (the OS speaks only in transcribed lines).
- Media 404 → state-frame poster fallback (thumbEl pattern), report in sysline.

## 9. [Describe] <change scenarios>

- **New archive** (different film): re-run states+text extraction; the OS
  re-infers itself — different machines, different verbs, same theory. Nothing
  in os.html names THE WORLD BEHIND THE EYES except data.
- **Richer grammar**: if later screens show new verbs (SCAN, LINK), the census
  picks them up; adding an [operation] = one entry in the VERBS table binding
  screen-verb → archive action. Theory unchanged (I1 still holds).

## 9b. [Measure] <the transcription census> (ground truth, not projection)

tesseract over all 520 state frames → **1,257 clean lines across 130 machines**
(`os_text.json`). The machines' own verb frequency:

| <verb> | count | licensed [operation] |
|---|---|---|
| RUN | 82 | [RUN chapter/act/film] — the dominant verb; the OS is an executor |
| MOVE | 20 | [MOVE x TO VIEW] — the viewing act |
| DEFINE | 13 | [DEFINE ch = title] — the naming act (binds monteTitles) |
| SYSTEM | 7 | [SYSTEM report] |
| LOAD | 4 | [LOAD archive] (the boot itself) |
| BOOT | 3 | power path |
| EYE | 1 | self-reference → report |
| SET | 1 | reserved (change scenario #2) |

Sample screen-lines now executable: `RUN CHAPTER 03`, `RUN ACT 01`,
`> MOVE IMAGE TO VIEW`, `WORLD EYE SYSTEM 14.3`,
`ACT III — THE WORLD COMES BACK AS DATA`.

## 10–12. <program text> · mapping · survival

Program text: `monte_os_text.py` (transcription), `os.html` v14.3 (shell +
manual pages + evidence lines). Mapping: <machine>=os_states.json boots ∪
os_text.json; [operations]=VERBS table in os.html, each row {verb, source pid,
action}; I1 is the table's construction rule; I2 is previewOf/playTrans; tests
= the live walk (power→run 5→move to view→halt). Residual human theory: the
*meaning* of a screen's diagram (□ inside ○) is not computable — the operator
reads it; DEFINE exists so the human can bind meaning the machine only draws.
