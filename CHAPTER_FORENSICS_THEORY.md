# CHAPTER FORENSICS + VIDEO-AS-CODE — theory-of-the-program

*A different problem from the OS. The OS trusts the archive; forensics distrusts
it. Built after WORLD_EYE_OS_THEORY.md; grounded in the dense OCR of every boot,
chapter by chapter (`MONTE/CHAPTER_DEEP/chNN.json`).*

---

## 1. [Elicit] <purpose>

Two joined problems the OS surfaced:

**(A) FORENSICS.** The OS filed each boot by its filename (`chNN_...`). But
some boots are in the wrong place, mislabeled, or are distinct operations
disguised as duplicates. The purpose: **sample every screen densely enough to
read all its text, then judge the archive by what the screens SAY, not what the
files are NAMED** — catching <imposters>, <misprints>, and <hidden-orders>.

**(B) VIDEO-AS-CODE.** The purpose: **make each clip legible as code** — a real
video frame re-drawn as a BEFLIX character field in a CRT, so the image and its
encoding are the same act. The boots draw the world in glyphs; the clips should
too.

The unifying principle is Bateson's: *information is a difference that makes a
difference.* Forensics separates meaningful difference (a boot announcing
another chapter) from noise (OCR jitter of one screen). Code-view turns a
continuous image into a field of discrete differences (glyph vs. glyph).

## 2. [Identify] <entities>

- <chapter> n — a filing bucket (1–14)
- <machine> — a boot, filed under a chapter by its filename
- <frame> — a dense sample (3 fps across the live span; ~30 per boot)
- <screen-line> — one OCR line of a <frame>
- <announced-chapter> — the chapter a screen's TEXT claims (`CHAPTER 16`)
- <filed-chapter> — the chapter the FILENAME claims (`ch03_...`)
- <imposter> := <machine> where announced ≠ filed (screen wins)
- <operation-class> — RUN | DEFINE | MOVE | LOAD | SYSTEM
- <difference-key> — normalized full text (`[^A-Z0-9]` stripped) — the identity
  of a screen for comparison; two machines with the same key are one operation
- <hidden-order> — the sequence of operation-classes / announced-chapters across
  a chapter's machines, read as an implied program
- <code-render> — a real <frame> quantized to a glyph field by a luminance <ramp>

## 3. [Define] <operations>

- [dense-sample] : <machine> → {<frame>} at 3 fps (was 4 total; now ~30)
- [ocr-all] : {<frame>} → per-frame <screen-line> sets (2× negate contrast)
- [announce-detect] : lines → <announced-chapter> votes (`CHAPTER n` / `CH nn`)
- [imposter-flag] : announced.mode ≠ filed → flag with vote evidence
- [dedupe] : lines → unique by <difference-key> (kills OCR-jitter false positives)
- [cluster] : machines → groups by <difference-key> — redundant vs. distinct
- [order-infer] : chapter's machines → [(pid, op, announced)] in filed order
- [encode] : <frame> pixels → glyph field: L = 0.299R+0.587G+0.114B, γ=0.85,
  glyph = ramp[round(L·(|ramp|−1))]; ramps DOTS/ASCII/HEX/BLOCKS

## 4. [Specify] <conditions>

- <live-span> = static − 0.2s [bounds] sampling (never OCR the frozen static).
- <served-origin> [enables] getImageData in code-view (file:// taints the canvas).
- <majority> [enables] imposter-flag: a single stray `CHAPTER n` vote is noise;
  a plurality that disagrees with the filing is the difference that matters.

## 5. [State] <invariants>

- **I-forensic** — when screen and filename disagree, the SCREEN is truth; the
  report always carries the vote evidence so a human can overrule.
- **I-difference** — a flagged difference must survive [dedupe]: OCR jitter of
  one screen must normalize to one <difference-key>, not many.
- **I-nondestructive** — forensics never rewrites loading.json; it emits a report
  the OS *reads*. Misfilings are shown, not silently corrected (a human decides).
- **I-encode-faithful** — code-view derives every glyph from a real pixel; no
  procedural fallback (that was the old synthetic dot-field; this is the frame).

## 6. [Map] <state transitions>

```
per chapter n:
  {machines} —[dense-sample]→ {frames} —[ocr-all]→ {lines/frame}
    —[announce-detect]→ votes —[imposter-flag]→ {imposters}
    —[dedupe]→ keys —[cluster]→ {redundant | distinct}
    —[order-infer]→ implied program
  → chNN.json  (OS reads it: ⚠ badge on imposters, full text in the manual)

per clip (code-view):
  <video> —(loadeddata / rAF)→ <frame> —[encode with ramp]→ glyph field → CRT
```

## 7. [Expose] <assumptions>

- <safe> 3 fps is dense enough to catch a changing screen (boots are ~10s;
  30 reads). <requires-user-decision> whether to go denser (fps 6) if a boot's
  text changes fast — cost is linear in OCR time.
- <uncertain> digit OCR confuses 0/8, 1/7, 6/16 — so imposter votes can be noisy
  (ch03's `16` also drew stray 10/18). Mitigation: report votes, flag as
  CANDIDATE, let the human confirm against the evidence frame.
- <safe> luminance ramps read as image because the eye integrates the field;
  <uncertain> whether ASCII vs DOTS reads better per clip — hence user-selectable.

## 8. [Find] <failure modes>

- All-noise OCR for a boot → announced = none → not flagged (silent, correct).
- A boot with NO chapter text (pure diagram) → announced = none → judged only by
  <operation-class> and cluster, never falsely flagged.
- Two boots identical by key → cluster collapses them → reported as redundant,
  not as two operations (prevents inflating "distinct operations").
- code-view on a protected/tainted source → "SIGNAL PROTECTED" instead of crash.

## 9. [Describe] <change scenarios>

- **Auto-refile**: if the user trusts the flags, an [apply] pass could rewrite a
  boot's `ch` to its announced chapter — but I-nondestructive says propose-only;
  the apply is a separate, explicit, reversible operation.
- **Deeper text**: raise fps, or add a second OCR engine (Vision.framework) and
  vote across engines — I-difference still gates via <difference-key>.
- **Code-as-edit**: the glyph field is data — a clip's code could be diffed
  against another's (two videos, one difference field), making montage itself a
  forensic operation.

## 10–12. <program text> · mapping · survival

`monte_chapter_deep.py` (forensics) · `os-code.html` (video-as-code). Mapping:
<imposter> = the `imposters[]` array (announced≠filed + votes); I-difference =
`norm()` + the cluster keys; [encode] = the `encode()` glyph loop; I-forensic =
report-only JSON the OS reads. Tests = the ch03 run (found the
`red_machine_at_future` → CHAPTER 16 imposter) + the live code-view of
"Programmers at workstations". Residual human theory: the OCR cannot tell a
*deliberate* cross-reference (a ch03 screen that legitimately points forward to
chapter 16) from a *misfiling* — only a human reading the evidence frame can say
whether an imposter is an error or an echo. The report exists to put that
judgment in front of a person, not to make it for them.
