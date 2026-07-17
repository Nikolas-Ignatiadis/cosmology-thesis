# Using AI in a Physics Thesis — A Practical Guide

*This is how I actually used AI assistants in my thesis ("Cosmological
Constraints and Dark Energy Reconstruction with Bayesian Evidence", University
of Ioannina, 2026). I wrote it for whoever takes over this line of work after
me. Full disclosure: I drafted this guide with AI help too, and then edited
it myself — which happens to be the exact workflow it describes, so consider
it a demo.*

---

## 1. What the AI did — and what it didn't

**The AI helped with:**
- **Building the pipeline.** I translated the group's inherited grid-scan code
  to Python myself (about halfway), then gave my translation to the AI and
  asked it to turn it into a Nautilus nested-sampling pipeline. This was the
  deepest AI involvement in the whole project. Section 4 explains what made
  it safe.
- **Writing.** Drafting chapters from my own notes and results.
- **Reviewing.** Acting as a hostile referee on the finished draft. It caught
  real problems: a methods section describing something my code didn't do, an
  inconsistent statistics scale, a wrong citation.
- **Boring but error-prone jobs.** Notation consistency, equation punctuation,
  BibTeX repair, LaTeX layout bugs, regenerating a figure from final values.
- **Debugging hints.** Most importantly: when the z† posterior misbehaved, the
  AI pointed me to the calibrated r_d formula. The hint was AI; the diagnosis
  and the reruns were mine.
- **Checking the literature** by fetching arXiv abstracts — never from its
  memory (see case study 6.2).

**The AI did NOT:**
- Make any decision. Which models, which data, which priors, what to conclude
  — all mine (and the supervisor's).
- Get trusted. AI-written code and AI-written text were treated the same way:
  useful drafts that must be verified before they count.
- Prepare my defense. If you can't explain every equation without the text in
  front of you, no workflow saves you.

## 2. How to organize your chats: one hub, many disposable workers

If you take one practical thing from this guide, take this.

**Keep one central chat — the "hub" — that knows everything about your
project.** All my decisions, final numbers, and conventions lived in one
long-running chat. I never wasted its context window on heavy work.

**For every heavy task, the hub writes a prompt for a fresh, disposable
chat.** That throwaway chat does the big job (review a 140-page PDF, edit ten
LaTeX files, interview you for personal additions), you take the output back
to the hub, and you never open the throwaway chat again.

Why this works:
- **The hub stays alive.** Long context makes chats slow, expensive, and
  forgetful. If your main chat also does the heavy lifting, you lose your
  project memory in a week. Mine lasted the entire thesis.
- **Disposable chats are cheap.** They burn usage, then they're gone. No
  cleanup, no confusion about which chat knows what.
- **It forces good prompts.** A fresh chat knows nothing, so the hub must
  pack the prompt with everything needed: the task, the ground-truth numbers,
  the conventions, the output format. This discipline alone prevents most
  AI errors — a model with the correct facts in front of it doesn't need to
  guess. (Templates in the Appendix.)
- **Outputs come back in a fixed format** (edit lists, "pending confirmation"
  drafts, verification reports), so the hub — and you — can check them.

My rule of thumb ended up being: anything that needs a big file upload or
more than a couple of rounds of back-and-forth goes to a disposable chat.
Anything that needs actual judgment about the project stays in the hub.

## 3. The ten rules

1. **Reproduce published results before trusting anything.** My pipeline had
   to match the published Akarsu et al. constraints before I believed any new
   number from it. And if your pipeline was written with AI help (mine
   was), you don't get to skip this: you didn't type every line yourself,
   so matching published results is the only trust you have.
2. **The notebook is the truth; the text only describes it.** If text and
   code can disagree, check the code and fix the text. Never the reverse.
3. **Sort every AI suggestion into three bins:** (A) mechanical — apply;
   (B) touches methods or science — AI drafts it, YOU verify against the
   notebook or the paper before it goes in; (C) numbers — verify only, never
   let the AI edit them.
4. **No number without a source.** Every number in the text must trace to a
   table, a chain, or a stored output. Audit AI-written text against a list
   of your final values.
5. **One source of truth per quantity.** My Figure 5.10 and Table 5.7
   disagreed in the second decimal because they came from different sessions.
   Fix: build both from the same final numbers, and add a warning to the
   plotting cell if live results drift.
6. **Version discipline.** Before every AI editing pass, upload the CURRENT
   files. I nearly lost work twice by running edits on stale versions.
7. **Ask for edits as exact old→new replacements, not rewritten files.** You
   can't review what you can't diff. Check brace balance and references after
   every pass.
8. **Literature questions get answered by fetching the paper,** never from
   the AI's memory — it fails in both directions (case study 6.2).
9. **Delegated prompts must be self-contained** — see Section 2.
10. **You must be able to defend every sentence.** Read each section aloud;
    any sentence you wouldn't say in the exam, rewrite yourself.

## 4. The riskiest step: AI-converted code

The project started with me handing the AI my half-finished Python
translation of the group's grid code and asking for a Nautilus version. If
you do the same (you will), three things are mandatory:

1. **Understand before running.** Every likelihood term, every prior, every
   conversion in the generated code — explainable by you, line by line.
2. **Reproduce before extending.** The pipeline earns trust by matching
   published results, not by looking right.
3. **Stress-test the seams.** Add guards and checks exactly where generated
   code hides subtle bugs: I added rejection of unphysical E²(z)<0 histories,
   varied the sign-switch smoothing (ν ∈ {10, 20, 50}), and computed every
   evidence over 5 random seeds because Nautilus gives no internal error bar.

## 5. The workflow, in order

1. **Science first.** Runs, debugging, decisions, supervisor meetings — and
   written notes of all of it. Everything downstream is only as good as these
   notes.
2. **Draft.** The hub (or a worker chat) drafts chapters from your notes:
   your decisions and reasons, your final numbers, the chapter's argument in
   bullets.
3. **Hostile review.** A disposable chat reviews the full PDF against your
   ground truth. Ask for problems explicitly, or you'll get compliments.
4. **Fix pass.** One disposable chat, one prompt, everything sorted A/B/C
   (Appendix A.1). B-items come back as "pending confirmation" — check them
   against the notebook before applying.
5. **Make it yours.** Two parts: (a) thin out the AI's writing tics —
   recurring "not X but Y" sentences, "It is worth noting", identical
   paragraph rhythm, dramatic chapter endings; (b) add short first-person
   passages about things only you know (how you found the bug, why you
   dropped a dataset). For these, the AI interviews YOU and only fixes
   grammar and accuracy (Appendix A.2). Don't let it write them.
6. **Supervisor round.** Split the correction list the same way: text jobs to
   the AI, anything needing chains or plots to you.

## 6. Case studies — read these even if you skip everything else

### 6.1 The likelihood that wasn't there
My draft described the SH0ES anchoring as a full 77-supernova vector
likelihood with covariance. Detailed, plausible, well-written — and my code
actually applied a simple Gaussian prior on M_B. Nothing in the text looked
wrong; only opening the notebook settled it. **Fluent prose is not evidence
of correctness.** That's why Rule 2 exists.

### 6.2 The AI's memory vs. the arXiv
During review, the AI insisted a cited paper had dropped the Planck
likelihood; my text said otherwise. Fetching the arXiv abstract showed MY
text was right and the AI's memory was wrong. A few days earlier, the same
fetch-and-check habit caught a real error in the other direction: a
bibliography entry whose title belonged to a completely different paper than
its arXiv ID. **Never settle a literature question from AI memory — in either
direction.**

### 6.3 The stale figure
A figure and its table disagreed in the second decimal: the figure PDF came
from an earlier run. Repair: rebuild the figure from the table's final values
and add a warning to the plotting cell. **In a 140-page document you will
not catch these by being careful. Set up single sources of truth so they
cannot happen.**

### 6.4 Sign conventions
The reference paper defines Δln Z with the opposite sign to mine. This caused
repeated confusion across notebooks, text, and AI sessions, until the flipped
quantity got its own symbol (Δln Z′). **State conventions in every prompt
that involves signed quantities, and give different conventions different
symbols.**

## 7. Where the AI reliably fails

- Literature facts from memory — fetch the source.
- Numerical consistency across a long document — audit against your list.
- Knowing what YOUR code does — it describes what similar code usually does,
  convincingly.
- Seeing its own writing tics — plan a dedicated pass to remove them.
- Disagreeing with you — ask for a hostile referee, or you get flattery.

## 8. Disclosure

Per the department's practice, the thesis text itself contains no AI
references; this document plus the public analysis repository are the
disclosure and the reproducibility record. Agree on this with the supervisor
EARLY — not at submission. The bottom line under any arrangement: the
analysis must be yours, verified independently of any AI, and you must be
able to defend every line.

---

## Why this guide is not a prompt archive

This project ran for six months across something like thirty separate
chats. The original idea was an appendix with "the prompts that worked", and
I dropped it, because prompts are not code. Each prompt worked because my hub
chat wrote it while carrying the state of the project at that exact moment:
which version of the files, which numbers were already validated, which
mistakes we had already made. Hand the same strings to a different student
with a different thesis (or even a different AI model) and they would be
useless — possibly worse than useless.

What actually transfers is the system that produced them. Put the ground
truth inside every delegated prompt. Say explicitly what the chat may change,
what it may only draft, and what it may only read. Require output you can
review. This guide *is* that system; the templates below just show its shape.
And if what you want is to reproduce my results, you don't need my
conversations at all — you need the notebooks sitting next to this file.

---

## Appendix — Prompt templates (from the actual project)

### A.1 Correction pass (the A/B/C prompt)
```
You are performing a post-review correction pass on the LaTeX sources of my
thesis. [context: topic, files, supervisor]
Your job is split into three strict categories:
- A. FIX DIRECTLY — mechanical, zero-judgment edits. Apply them. [list]
- B. DRAFT & FLAG — write the replacement text, but I confirm against my
  notebooks or the literature before applying. Insert nothing silently. [list]
- C. VERIFY & REPORT ONLY — cross-check these numbers against the canonical
  values below; change nothing. [your final values]
Do not rewrite prose style. Do not touch scientific claims beyond this list.
OUTPUT: (1) per-file old→new edits; (2) a "PENDING YOUR CONFIRMATION" section
with the B drafts; (3) a verification report, PASS/MISMATCH per value.
```

### A.2 Personalization interview
```
The thesis text is finished. What remains is adding short first-person
remarks at specific points — moments from the research that only I know.
You must NOT ghostwrite these. Go one item at a time: ask me 2-3 questions to
get the story in my own words, then ask ME to draft the sentences. Intervene
only on factual accuracy (against the facts below), grammar, and register.
[per item: target section + the facts]
End product: a handoff block with FILE / SECTION / exact ANCHOR sentence /
final approved TEXT, for another chat to insert verbatim.
```

### A.3 Hostile review
```
Review this thesis as a hostile referee. Separate findings into:
(a) content errors and internal inconsistencies — check every number against
    the values below;
(b) prose problems;
(c) things you cannot decide without my code or data — list these as
    questions for me, do not guess.
[your final values, conventions, key project decisions]
```


## Appendix B — A real, complete example

This is (lightly condensed) an actual prompt from this project. Context: a
review had found ~14 issues in the thesis. The hub chat — which knew the
project's canonical numbers and every decision — packaged them into one
prompt for a disposable chat, sorting each item into A/B/C.

**What the hub produced:**

```
You are performing a post-peer-review correction pass on the LaTeX sources
of my undergraduate cosmology thesis (University of Ioannina, supervisor
L. Perivolaropoulos). Files: 01_introduction.tex ... 08_conclusions.tex,
appendices, references.bib.

Your job is split into three strict categories:
- A. FIX DIRECTLY — mechanical, zero-judgment edits. Apply them.
- B. DRAFT & FLAG — write the correction, but I must confirm against my
  notebooks or the literature before applying. Insert nothing silently.
- C. VERIFY & REPORT ONLY — cross-checks. Change nothing; report mismatches.

Do not rewrite prose style or voice (I will do that myself), and preserve
the existing LaTeX conventions (booktabs tables, figure [H] placement).

## A. FIX DIRECTLY
A1. Spelling: Chapter 6 is in British English (favoured, localised,
    artefact) while the rest is American. Convert Chapter 6 to American.
    Do NOT alter spellings inside bibliography entries or quoted titles.
A2. Jeffreys scale (Table 4.1): labels are shifted one level too strong.
    Correct to Trotta (2008): |ΔlnZ| ≈1 weak, ≈2.5 moderate, ≈5 strong.
    Then sweep Chapters 5, 6, 8 and the Abstract so every verbal label
    matches: ΔlnZ = +1.38/+1.39 → "weak"; ≈ −3.4 → "moderate"; [...]
[...more A items...]

## B. DRAFT & FLAG
B1. §4.4.5 describes the anchored fits as adding a full 77-calibrator
    likelihood with a 77×77 covariance. My records say the pipeline instead
    applies a Gaussian prior M_B = −19.2435 ± 0.0373. Draft the corrected
    section, but FLAG it: I will confirm against the notebook which of the
    two is actually implemented before applying.
[...more B items...]

## C. VERIFY & REPORT ONLY
C1. Cross-check every number in Tables 5.1–5.7, 6.1–6.3, 7.1 against this
    canonical list; report any mismatch:
    - ΛCDM (PP/DES): Ωm 0.3026/0.3029, h 0.6854/0.6851
    - Evidence vs ΛCDM: wCDM −3.37/−3.26; ΛsCDM +1.38/+1.39
    - Isolation (ΔlnZ): 3D BAO +2.37…+2.77; BAOtr +13.77…+15.94
    [...full list of final values...]

## OUTPUT FORMAT
1. Per-file list of applied edits (category A), as exact old→new snippets.
2. "PENDING YOUR CONFIRMATION" section with the B drafts.
3. "VERIFICATION REPORT" for C: checked values, PASS/MISMATCH.
```

**What came back:** the A edits applied, the B items as drafts marked
pending, and a verification table.

**What the human then did:** opened the notebook, checked which SH0ES
implementation actually runs (two minutes), confirmed or corrected the B1
draft, and pasted only the approved text back. The disposable chat was never
opened again; the outcome (one line: "B1 confirmed: Gaussian prior; section
replaced") was recorded in the hub.

Why does this prompt work? Mostly because the fresh chat is never allowed
to guess. It gets the actual numbers and conventions instead of relying on
memory, it is told exactly what it may change on its own and what it may only
propose, and it has to hand back its work in a form I can check line by line.
Go through the failure cases in Section 6 and you'll see each one would have
been stopped by one of these.
