# EL DESIERTO

> *"For me the world is weird because it is stupendous, awesome, mysterious, unfathomable; my interest has been to convince you that you must assume responsibility for being here, in this marvelous world, in this marvelous desert, in this marvelous time."*
> — Don Juan Matus, *Journey to Ixtlan*

**EL DESIERTO is a research project about making meaning and narrative from an abundance of machine-generated video.** The desert is the abundance: thousands of AI-generated clips that cost almost nothing to summon and, left alone, mean almost nothing. The work is the responsibility — to see the desert instead of drowning in it, and to make something that says something before generating forever.

Live: **https://hartswf0.github.io/el-desierto/** — open [`hub.html`](hub.html) for the full instrument index.

---

## The turn this project is built on

Generative video collapses the cost of footage to near zero. That quietly breaks the interface we edit with. **The timeline is an instrument for scarcity** — you shoot precious, limited footage and you cut it *down*. When footage is abundant, cutting-down is the wrong verb and the timeline is the wrong metaphor.

EL DESIERTO proposes the inverse:

> **A film over machine abundance is not a timeline. It is a field you sample. Assembly is sampling, not cutting.**

Every tool here is a different way to draw a film out of a field. Not features of one app — a **family of narrative interfaces**, each a hypothesis about how meaning is made from too much.

---

## THE WORLD BEHIND THE EYES

The deepest instance is a feature-length essay-film: a secret history of the machines placed between the eye and the world — 3D cinema, the Telesphere mask, Sensorama, Sutherland's Ultimate Display, the Sword of Damocles, on through Oculus and Vision Pro. Fourteen chapters, one per era, each scored to its own track from the album *Paper Glasses, Electric Wonder*.

The recursion is the point: **it uses the newest apparatus that stands between us and the world — generative video — to tell the history of every apparatus that came before it.** Media archaeology performed by the medium it studies. The archive stays honest about this — every clip carries a status: `documented`, `inferred`, `speculative`, or `counterfactual`. 105 of 562 records are labeled AI reconstructions of a history that may not have happened exactly so. Nothing pretends to be found footage.

---

## MONTE — the base

**MONTE is the substrate: a served archive plus a set of laws plus a pure sampler.** It is reusable — swap the archive, keep the machinery, make a different film. It is *not* the research; it is the ground the research stands on.

The corpus (in [`MONTE/`](MONTE/)):

| | |
|---|---|
| **14 eras** | 3D Golden → Telesphere → Sensorama → Ultimate Display → Damocles → Public Grid → NASA View → VPL → Arcade Era → VR Winter → Oculus → Consumer VR → Open Standard → Vision Pro |
| **373 video clips** | each transcribed, tagged, and given a historical status |
| **130 boot screens** | terminal recordings — the machine's own parallel consciousness, used as titles and connective tissue |
| **14 songs** | one per era, with offline audio-maps (energy, onsets, phrase boundaries, quiet windows) |
| **milestones** | the media-archaeological claim behind each era — actors, institutions, the thesis |

Data layer, all fetched at runtime so every instrument runs over the same truth:
`monte_manifest.json` · `manifest2.json` · `monte_transcripts.json` · `milestones.json` · `loading.json` · `audiomaps/`

### The laws

Abundance needs rules, or it stays noise. The sampler enforces them so any cut is defensible:

- **Sentence law** — never cut a spoken word; narration only where a whole line fits.
- **Lip-sync law** — a talking head must never mouth silently; if you see the mouth, you hear the voice.
- **Coverage invariants** — a film uses every unique clip before it repeats; the archive is exhausted, not sampled lazily.
- **Boot-static law** — loading screens play to their calibrated static and cut there, never mid-freeze.
- **Duck envelopes** — the song ducks under every spoken line, in the browser and in the renderer, by the same math.

---

## The instruments

Each is a different answer to *how do you make a film from too much footage?* All run over the same MONTE base.

- **[GEL](gel.html)** — the mixing wall. Every film at once, scroll vertical and horizontal, the whole desert visible; edit stacks like a genome.
- **[MONTE — The Instrument](monte.html)** — the full editor. Eras as lanes, a present-moment read-head, direct drag-and-drop, a chapter rail to watch the whole film or tap any part, and a **TITLES** pass to name each chapter to the footage before you bake.
- **[GEL MONTE — the film](gel-monte-clean.html)** — press play, watch it through: title, prologue, acts, every chapter opening on its full boot, straight through with the score.
- **[MONTE BREEDER](monte-breeder.html)** — the essay-film as genetics. Set a profile, generate 20 worlds, breed and fork them; a chosen genetic chain compiles into a real-media EDL that plays actual clips + song + transcript captions.
- **[MONTE SPIN](monte-spin.html)** — counterfactual montage. Draw one clip per lane — a Monte-Carlo cut — keep the ones that hold, cascade them into versions.
- **[MONTE GENOME](monte-genome-spin.html)** — the film as a diploid genome. Four loci are four eras, each allele a real clip; dominance and co-expression decide which shot shows; SPIN recombines, EVOLVE breeds.
- **[GEL SCORE](gel-score.html)** · **[GEL BOOTS](gel-boots.html)** · **[FEEDWALL](feedwall.html)** · **[WORLD OP-3](world-op3.html)** — the reading room, the loading-screen archive, the feed wall, the bridge.

---

## The pipeline: pure sampler, render twin

The heart is a **pure sampler** — deterministic JavaScript over the published JSON. The same function that draws a cut in the browser can be lifted, unchanged, into Node or a Python renderer. Seed in, EDL out; the EDL is the contract.

```
14 eras + album ─► manifest / transcripts / audio-maps / boot calibration
        │
        ▼
   pure sampler (laws: sentence, lip-sync, coverage, boot-static)
        │
   ┌────┴────┐
   ▼         ▼
browser    ffmpeg twin  ── identical cuts, identical duck envelopes
compositor  (monte_render2 · monte_program2film · monte_weave)
        │
        ▼
   watch it → if it reads right → bake the MP4
```

You defer the commitment — Monte Carlo, branches, breeding — until you *watch* and it holds. Then you bake. **The projectionist and the renderer play the same structure**, so what you saw is what you print.

---

## What the research is actually about

MONTE makes a film. EL DESIERTO asks what MONTE is an instance of. Three questions it treats as real:

**A new video motion language.** These clips are not images — they are motion, and machine-made motion has a grammar. The archive taxonomizes it (audience, portrait, eye, machine, reels, screen, scope, wireframe, document, city) and, further, the clips *speak*: Whisper transcribes what the generated footage says, and that line becomes evidence of what a shot shows. A generated 1952 audience says *"A new dimension in cinema thrills."* The machine dreams dialogue; we read it as content.

**New models of making meaning with machines.** The material is machine-made all the way down — generated video, machine-transcribed, machine-scored, interleaved with recordings of machine boot screens — and the *edit itself* becomes machine-legible: the transcript-as-evidence, the genome-as-cut, sampling-as-assembly, the labeled real/fabricated status. Meaning is not imposed on the footage; it is *found and declared* through the interface.

**Abundance as a new problem, with new instruments.** Scarcity was solved by the cut. Abundance raises different problems, and each got an instrument: *how do you see thousands of clips?* (the loupe, the read-head, the present-moment line across lanes); *how do you not repeat?* (coverage invariants); *how do you find the one that fits?* (FIND SIMILAR, genome search); *how do you commit?* (defer until it reads right, then bake). The final cut becomes a **curation** problem, not a construction one.

---

## MONTE base vs. the research repo

- **MONTE (base):** the corpus + laws + sampler. A reusable engine for making *one* film from an abundant machine archive. Portable — point it at a different archive and it makes a different film.
- **EL DESIERTO (research):** the inquiry the engine is an instance of — a study of **narrative interfaces for machine abundance**, and a working document of a new motion language and new ways of meaning-making with machines. The instruments are the argument.

---

## Run it

Everything is a single self-contained HTML file over the served `MONTE/` archive — no build step. Because they `fetch` the archive, they must be **served next to `MONTE/`**, not opened as `file://` (a `file://` origin CORS-blocks the fetch and the page comes up blank).

```bash
cd el-desierto
python3 -m http.server 8749
# open http://localhost:8749/hub.html
```

The Python render twins live in the research repo (`monte_render2.py`, `monte_program2film.py`, `monte_weave.py`, `monte_hybrids.js`, `monte_punnett.js`) and need `ffmpeg`. Media in this repo is 360p previews; full-quality finals live off-repo (see `finals_urls.json`).

---

## The desert

The clips are stupendous, awesome, mysterious, unfathomable — and there are too many. That is the condition, not the flaw. These instruments exist to help you assume responsibility for being here, in this marvelous desert of machine images: to see it, to sample it with intention, to name what you show, and to make a film that means something — in this marvelous time.
