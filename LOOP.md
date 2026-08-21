# LOOP.md — Assignment double-check

A recurring pre-submission review of the lab currently being worked on.
Run it with the `/loop` skill:

```text
/loop 20m follow the task in LOOP.md
/loop follow the task in LOOP.md     # let the model pace itself
```

**This task reports. It does not fix.** Never edit a notebook during a loop run —
the student decides what to change. Report findings and stop.

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

Sub-bullets count as separate questions. They are the ones most often missed.

### 3. Check each question

For each one, classify:

| Status | Meaning |
| --- | --- |
| **answered** | a following cell clearly addresses it, with output |
| **partial** | started but incomplete — e.g. `Sex` counted but not `Survived`, code with no output, a stub like `#To do` |
| **missing** | no cell addresses it at all |
| **placeholder** | literal `...`, `TODO`, `#To do`, or an empty answer slot |

Match answers to questions by position and content, not by guessing. If a cell's
purpose is ambiguous, call it ambiguous rather than assuming it counts.

### 4. Run the mechanical checks

- **Header** — student name and ID filled in, not `...`.
- **Filename** — matches the submission rule stated in the notebook
  (Lab 1: `Lab1_name.ipynb`).
- **Execution order** — cells numbered `1..n` top to bottom. Out-of-order or `[ ]`
  counts mean the notebook was not restarted-and-run-all before saving.
- **Outputs present** — a code cell answering a question but showing no output is
  a partial answer.
- **Errors** — any cell with a traceback in its output.
- **Reproducibility** — no reliance on a variable defined in a cell that was later
  deleted or edited; no hard-coded absolute paths.
- **Cleaning claims match the data** — if a cell says it filled or dropped something,
  the printed before/after actually shows that.
- **Reflective answers** — present and in the student's own words. Flag if empty;
  never write them.

### 5. Report

Short, scannable, most-blocking first:

```text
Lab: W1/Lab1_Introduction.ipynb        (checked HH:MM)
Blocking:  E, H, I, J unanswered · name/ID still "..."
Partial:   D — counted Sex, not Survived
Clean:     A B C F G K L, no tracebacks, cells in order
```

Then, for each blocking or partial item, **one line** naming the question, the cell
index, and what is missing — as a hint, not an answer:

```text
E (after cell 17) — needs the survived/did-not-survive counts; `Survived` is 0/1,
                    so value_counts() on it answers this directly.
```

Do not include working code in a loop report. If the student then asks how to do one,
switch to teaching-assistant mode per `CLAUDE.md`.

### 6. Quiet runs

If nothing has changed since the previous run — same notebook, same statuses — say so
in one line ("no change since HH:MM, still 4 unanswered") and mark the tick as no-op.
Do not re-print the full report every time.

Announce loudly when the notebook first reaches: every question answered, header
filled, no tracebacks, cells in order. That is the "ready to submit" signal, and it is
worth interrupting for.

---

## Known open items (W1, verified 2026-08-21, 4th pass)

Refresh these on each run; they are a starting point, not the truth.

- **C (sub)** — rows with at least one missing value: not answered
- **C (sub)** — how to drop those rows: not answered (`dropna` absent)
- **Header** — student name and ID still `...`
- **Filename** — still `Lab1_Introduction.ipynb`, rule asks for `Lab1_name.ipynb`
- **Execution** — E and the new L cell have no output yet; needs Restart & Run All,
  which also renumbers the counts (currently 190-207 with nulls) to 1-21
- **Cell 18** — bare `data` dump, answers nothing
- **Cell 27** — `by_port` assigned but never displayed

Fixed in the 4th pass (written for the student on request): **E** now counts
`data['Survived']` directly instead of grouping by sex; **L** regained the ``
word-boundary cell and the conclusion markdown now explains why Jackson / Ambrose /
Rosen do not count. Both were verified against the cached dataset before insertion.
Earlier passes fixed H, I, J and the missing outputs.
