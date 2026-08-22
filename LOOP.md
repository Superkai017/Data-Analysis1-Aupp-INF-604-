# LOOP.md — Answer-to-question check

A recurring check of the lab currently being worked on. One job: **for every question,
does the answer that follows actually answer that question?**
Run it with the `/loop` skill:

```text
/loop 20m follow the task in LOOP.md
/loop follow the task in LOOP.md     # let the model pace itself
```

**This task reports. It does not fix.** Never edit a notebook during a loop run —
the student decides what to change. Report the mismatches and stop. If the student
then asks for help on one, that is a normal request: switch to assist mode per
`CLAUDE.md` and answer it fully, code included.

**Not in scope:** no percentage grade, no marks for presentation, and no grading of
the reflective / opinion answers — those are the student's own thinking. Skip them.

---

## Each run

### 1. Pick the target

The most recently modified `W*/Lab*.ipynb` (`git status` shows what is in progress).
If several are modified, take the one with the newest mtime and name it in the report.

### 2. Pair each question with its answer

Walk the notebook in order. For every lettered question (**A**, **B**, **C**, …) and
every sub-bullet that asks something ("What should you do with column `Cabin`?"), find
the cell or cells that follow it and are meant to answer it.

Sub-bullets are questions too. They are the ones most often missed.

### 3. Give each question one verdict

| Verdict | Meaning |
| --- | --- |
| **match** | the cell answers the question that was asked, the output is there, and the claim is supported by the data |
| **partly** | answers only part of it — one sub-bullet done, the other not |
| **mismatch** | answers a different question than the one asked, or states a conclusion its own output contradicts |
| **not answered** | no cell addresses it, or it is a placeholder (`...`, `TODO`, `#To do`) |

**mismatch leads the report.** An answer that contradicts the output printed directly
above it is worse than a blank, because the student does not know it is wrong.

Rules that keep the check honest:

- **Verify before calling anything wrong.** Run the cell or check the dataset against
  the claim. Never from memory, never from what the answer looks like it should be.
- Match answers to questions by position and content. If a cell's purpose is
  ambiguous, say so rather than guessing either way.
- Judge the answer that is there, not the one you would have written. A correct result
  reached a different way is a match.
- Style (`inplace=True`, a dead variable, a bare `data` dump) is not a mismatch. At
  most it is a one-line note at the end.
- A cell with no output, or with an output that no longer belongs to its code because
  the code was edited after running, cannot support its claim — say so under that
  question.

### 4. Report

Short, mismatches first:

```text
Lab: W1/Lab1_Introduction.ipynb        (checked HH:MM)
10 of 12 match · 1 mismatch · 1 partly
```

Then **one line per problem**, naming the question, the cell index, and what is off —
as a hint, not an answer:

```text
E (after cell 17) — needs the survived/did-not-survive counts; `Survived` is 0/1,
                    so value_counts() on it answers this directly.
```

Do not include working code in a loop report. That is what assist mode is for, and the
student has to ask for it.

### 5. Quiet runs

If nothing has changed since the previous run, say so in one line ("no change since
HH:MM, still 1 mismatch") and mark the tick as no-op. Do not re-print the full list
every time.

Announce loudly the first time every question matches. That is the signal worth
interrupting for.

---

## Current standing (W1, checked 2026-08-22, 10th pass)

Refresh this on each run; it is a starting point, not the truth.

```text
Lab: W1/Lab1_ArtOudom.ipynb        (checked 21:03)
12 of 12 match - no mismatches, no unanswered questions, no tracebacks
```

Every answer now matches its question and every number is confirmed against the
cached CSV:

**A** 891 x 12 · **B** all 12 columns described · **C** 708 rows with at least one
missing value, `Cabin` 77% missing and dropped, `dropna()` leaves (712, 11) ·
**D** 577 male / 314 female · **E** 342 survived, 549 not · **F** 24 under 3, 22 over
60 · **G** 168 C, 77 Q, 646 S · **H** 216 / 184 / 491 · **I** 136 / 87 / 119 ·
**J** 233 female, 109 male · **K** 93 C, 30 Q, 219 S · **L** 0 named Jack, 0 named
Rose - the three hits are Jackson, Ambrose and Rosen.

The whole notebook ran in one top-to-bottom sweep with no gaps and no errors, so every
output belongs to the code above it.

Notes, none of them mismatches:

- renamed to `Lab1_ArtOudom.ipynb`, which satisfies the `Lab1_name.ipynb` rule in
  cell 1
- execution counts run 24-46 rather than 1-23: Run All on a live kernel, not a restart.
  The order is clean, so nothing is stale, but a restart would make that obvious to a
  reader
- cell 15 drops `Cabin` with `inplace=True` and prints nothing
- cell 20 is a bare `data` dump that answers nothing
- cell 31 assigns `by_port` and never displays it - the crosstab is what answers K
