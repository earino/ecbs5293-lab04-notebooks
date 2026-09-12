# Lab 4 — The notebook lies

**ECBS5293 — Computing for Analytical Work · Session 2, Block 4**

## Goal

`notebooks/clean_and_plot.ipynb` loads the sales data, drops rows with missing units, computes revenue per region, and writes `output/summary.csv`. When you are done it runs **top to bottom after Restart Kernel and Run All** and the CSV exists.

## How to run

In your terminal (Git Bash on Windows, Terminal on macOS), in the folder where you keep course work:

```bash
git clone https://github.com/earino/ecbs5293-lab04-notebooks.git
cd ecbs5293-lab04-notebooks
uv sync
```

Open **this folder** in VS Code (*File → Open Folder…*; if VS Code asks whether you trust the authors, choose **Yes** — in Restricted Mode the kernel picker lists nothing), open the notebook, and select the **`.venv` kernel** in the kernel picker — never a global Python or Anaconda kernel. Verify with `import sys; sys.executable` in a cell: the path must point inside this project's `.venv/`. (Alternative: `uv run jupyter lab` from this folder.)

Two controls you will need. To **move** a cell: drag it by its left edge, or Alt+↑ / Alt+↓ (Option+↑ / ↓ on a Mac). To **delete** a cell: the bin icon on the cell's hover toolbar. Ctrl+Z / Cmd+Z undoes. Moving a cell is a different action from editing its text.

Every cell already shows output. Do not trust it.

## What is broken

Nothing looks broken. Restart the kernel and run every cell from the top (VS Code: **Restart**, then **Run All**, in the notebook toolbar; JupyterLab: **Kernel → Restart Kernel and Run All Cells**). Then read the first error and ask: where did that variable come from the last time this "worked"?

## What you need to produce

1. The notebook passing Restart-and-Run-All, top to bottom, **twice** (the second run shows the first was not luck), with `output/summary.csv` written.
2. A commit of the working notebook with a message that says what you fixed (not "fix"). `git status` afterwards says *nothing to commit, working tree clean*.
3. **Recovery practised** (five minutes, after the commit): delete two cells from the notebook and save — do **not** `git add` it. `git status` shows it modified. `git restore notebooks/clean_and_plot.ipynb`. `git status` is clean again. Reopen the notebook and run it clean once more. One line in `DIAGNOSIS.md` part 5 saying you did this and what `git status` showed before and after. Homework 2 grades this move on your own.
4. `DIAGNOSIS.md` — all five parts; part 3 is the `NameError` traceback you got on the clean run.
5. **Explain it to a neighbour**, in the last ten minutes (the slide tells you when): symptom, cause, evidence, change, verification, pointing at your screen, not reading the note. Your neighbour asks the three questions on the slide, then you swap. Unsure, or you two disagree? Hands up, and one of us comes to you first.
6. **Lab checkpoint on Moodle**, before you leave: upload the `DIAGNOSIS.md` from your project folder, with its first line filled in (who you explained to, what you need help with).

## Rules

- Fix the *cause*, not the symptom. Do not hard-code a path to your own machine.
- You may use AI to explain errors and suggest what to inspect. You must be able to explain every change you make — your neighbour will ask you to at the end of the lab, in about a minute, without notes, and staff listen in.

## Hints, if stuck

1. The saved outputs are from a *previous* session's kernel, which had run cells in a different order. They prove nothing about this order.
2. `NameError: name 'X' is not defined` on a clean run means X was created by a cell that runs *later* (or by a cell someone deleted).
3. Reorder so every name is defined before it is used. Imports first. Then restart and run all — again.

## Diagnosis note

`DIAGNOSIS.md`.

## Stretch task

Move the load-clean-summarize logic into `scripts/summarize.py` so it can run fresh with one command, and add one sanity check (`assert df_clean["units"].notna().all()`). A script starts fresh every time; that is the point of the exercise.

## The last ten minutes

Finished or not, at minute 33 you turn to the person next to you (three if the row is odd). One of you explains, about a minute: what failed, why, the evidence that showed you, what you changed, how you know it works. Point at the screen; do not read the note. The other asks:

1. Show me the evidence — the raw output that told you the cause.
2. Why did it fail, not just where?
3. The what-if question on the slide.

Then swap. If either of you is unsure, or you disagree, put a hand up: staff come to you first. Then the answer to the what-if, for everyone. An unfinished repair is explained the same way — what you found so far.

Before you leave: the lab's **checkpoint on Moodle** — upload the `DIAGNOSIS.md` from your project folder with its first line filled in. That is what "complete" means; nobody signs you off.

## If you got lost: how to reset

Both of these **destroy work**. Read before running.

**Discard uncommitted changes (destructive)** — throw away edits and new files; keep your commits:

```bash
git restore --staged --worktree .    # every tracked file back to the last commit, staged or not
git clean -fd                        # and remove new, untracked files
```

> ⚠️ Permanently deletes uncommitted changes — staged or not — and any new untracked files.

**Full reset to the starter state (destructive)** — back to exactly what you cloned; throws away your commits too:

```bash
git reset --hard origin/main
git clean -fdx
```

> ⚠️ Discards your local commits and uncommitted changes. The `-x` also removes ignored files — `output/`, the `.venv/` environment — so the folder truly matches a fresh clone (`uv sync` rebuilds the environment in a minute). Without `-x`, leftover generated files can hide the very failure the lab wants you to meet again.
