# LOOP.md — Assignment grading pass

A recurring **grade** of the lab currently being worked on: mark it the way the
instructor will, report the estimated score, and name what is costing marks.
Run it with the `/loop` skill:

```text
/loop 20m follow the task in LOOP.md
/loop follow the task in LOOP.md     # let the model pace itself
```

**This task grades. It does not fix.** Never edit a notebook during a loop run —
the student decides what to change. Report the grade and the deductions, then stop.
If the student then asks for help on any item, that is a normal request: switch to
assist mode per `CLAUDE.md` and answer it fully, code included.

**The grade is an estimate**, not an official mark. Say so every time.

---

## Each run

### 1. Pick the target

The most recently modified `W*/Lab*.ipynb` (`git status` shows what is in progress).
If several are modified, take the one with the newest mtime and name it in the report.

### 2. Build the question list

Read every markdown cell and pull out:

- numbered sections (`## 1.`, `### 2.1.`, …)
- every lettered question (`**A.**`, `**B.**`, … through the end of the notebook)
- every sub-bullet that asks something ("What should you do with column `Cabin`?")

Sub-bullets are graded items, not decoration. They are the ones most often missed.

### 3. Mark each question

| Mark | Meaning |
| --- | --- |
| **1.0** | answered: code runs, output is present, and the answer is stated or obvious from that output |
| **0.5** | partial: only part of the question answered, or code with no output, or an answer asserted but not supported by the printed numbers |
| **0.0** | missing, a placeholder (`...`, `TODO`, `#To do`), or **contradicted by its own output** |

A letter with sub-bullets is worth 1.0 in total; its parts split it equally.

Rules that keep the marking honest:

- **Verify before deducting.** Run the cell or check the dataset. Never mark from memory.
- Match answers to questions by position and content. If a cell's purpose is ambiguous,
  say so and mark it 0.5 rather than guessing either way.
- Grade the answer that is there, not the one you would have written. A correct result
  reached a different way is full marks.
- Style problems (`inplace=True`, a dead variable, a bare `data` dump) are **notes**,
  not deductions — unless they hide the answer, which is a presentation deduction.
- An answer that contradicts the output printed directly above it is the worst failure
  mode in a lab: it scores 0 and leads the report.

### 4. Mark presentation and reproducibility

20 points, 4 each:

- **Header** — student name and ID filled in, not `...`.
- **Filename** — matches the submission rule stated in the notebook
  (Lab 1: `Lab1_name.ipynb`).
- **Execution order** — cells numbered `1..n` top to bottom. Out-of-order or `[ ]`
  counts mean the notebook was not restarted-and-run-all before saving. Partial credit
  if the body is in order and only a trailing cell is off.
- **Outputs present** — every cell that answers something shows its output; cleaning
  steps print the before/after so the effect is visible.
- **Runs clean** — no tracebacks, no hard-coded absolute paths, no reliance on a
  variable from a cell that was later deleted or edited.

### 5. Weights

| Component | Weight |
| --- | --- |
| Lettered questions (A, B, C, …) | 70% — split equally across the letters |
| Reflective / open sections | 10% |
| Presentation and reproducibility | 20% |

Reflective answers are marked on whether all the prompts are addressed and whether the
reasoning is there — never on whether you agree with them, and never by writing them.

Bands: **A** 90+ · **B** 80+ · **C** 70+ · **D** 60+ · **F** below 60.

### 6. Report

Short, scannable, grade first, then the biggest deduction:

```text
Lab: W1/Lab1_Introduction.ipynb        (graded HH:MM)
Grade (estimate): 77% — C

Questions    10.5 / 12 letters -> 61.3 / 70   (C 0.5, L 0)
Reflective    7 / 10
Presentation  9 / 20   (name/ID, filename, run order, silent cleaning cell)
```

Then **one line per deduction**, naming the question, the cell index, and what is
missing — as a hint, not an answer:

```text
E (after cell 17) — needs the survived/did-not-survive counts; `Survived` is 0/1,
                    so value_counts() on it answers this directly.
```

Do not include working code in a loop report. That is what assist mode is for, and the
student has to ask for it.

### 7. Quiet runs

If nothing has changed since the previous run — same notebook, same grade — say so in
one line ("no change since HH:MM, still 77%") and mark the tick as no-op. Do not
re-print the full breakdown every time.

Announce loudly when the grade first reaches full marks on the questions with the
header filled, the file renamed, no tracebacks and the cells in order. That is the
"ready to submit" signal, and it is worth interrupting for.

---

## Current standing (W1, graded 2026-08-22, 5th pass)

Refresh this on each run; it is a starting point, not the truth.

```text
Lab: W1/Lab1_Introduction.ipynb        (graded 20:34)
Grade (estimate): 77% — C

Questions    10.5 / 12 letters -> 61.3 / 70
Reflective    7 / 10
Presentation  9 / 20
```

Deductions, biggest first:

- **L — 0.0** (cells 29-30). The word-boundary cell was replaced by a copy of cell 28
  (plain `'Jack'` / `'Rose'`), so it prints 1 and 2 under the label "actually named",
  and cell 30 concludes there are two Roses and one Jack. The rows printed directly
  above it are `Brewe, Dr. Arthur Jackson`, `Hood, Mr. Ambrose Jr` and
  `Aks, Mrs. Sam (Leah Rosen)` — substrings, not names. Verified against the cached
  CSV: a word-boundary match returns 0 and 0. The answer contradicts its own output.
- **C — 0.5** (after cell 11). `data.isna().sum()` gives per-column counts; the
  question asks how many *rows* hold at least one missing value, and how those rows
  would be dropped. `dropna` never appears. (Row counts: 708 before `Cabin` is
  dropped, 179 after.)
- **Reflective — 7/10** (cell 3). All five prompts addressed in the student's own
  words, but one line each and no reasoning for the sample size or the data choices.
- **Presentation — 9/20**: name and ID still `...` (0/4); file still named
  `Lab1_Introduction.ipynb` against the `Lab1_name.ipynb` rule (0/4); cells 1-20 in
  order but the last cell is 23 (2/4); cell 14 drops `Cabin` with `inplace=True` and
  prints nothing, so the effect is invisible (3/4); runs clean, no tracebacks (4/4).

Notes, not deductions: cell 18 is a bare `data` dump that answers nothing; cell 27
assigns `by_port` and never displays it (the crosstab is what answers K).

Since the 4th pass: the student re-ran everything, so counts went from 190-231 to 1-20
and every answering cell now shows output — that lifted presentation and closed the
old "no output" deductions on E and L. L itself regressed: the 4th-pass word-boundary
cell and its explanation were overwritten. Earlier passes fixed H, I and J.
