# Changelog

Each skill keeps its own version.

## load-bearing-prompts

### 1.0.0 — 2026-11-14

First version in this repo.

- Input slot: the placement rule now puts the input data in its own labeled block and names it in the opening line.
- New rule: when a program reads the output, show the exact full shape once, as the last order.
- Two commentary sentences rewritten as plain statements.
- Worked example updated to show the labeled input block.
- Added `license` and `version` to the frontmatter.

## lazy-librarian

### 1.4.0 — 2026-11-18

Lighter to read, and tested on mixed tasks.

- Step 8's rules and "When a fix fails twice" moved into their own files, `prompts.md` and `fix-twice.md`, read only when their condition is met. The always-read core went from about 11,700 to about 9,700 characters.
- Destructive changes: assess recoverability. When version control, a backup or a holding folder can restore it, proceed and record the recovery path; otherwise stop and ask. "Always keep" keeps the stop for publishing, deploying and secrets. (Our test: 4 of 4 Haiku runs reasoned about recovery out loud, and none lost data, including on files git had never tracked.)
- From Félix's benchmark of 1.3.0 (12 Haiku runs): software is installed into the project's own environment when it has one, and anything put on the machine is listed with how to remove it and what came with it; a bug's check must fail for the reason the fix removes; a reused live thing (a login, a port, a running process) has its owner named; an unattended change names where its failures land and who will see them.
- Report sized to the change; defects outside the task get listed, not fixed; "the layers the design calls for" now reads "the layers the task needs".

### 1.3.0 — 2026-11-17

From a stress test by Anne and Chadrien of House of Solance (issue #1), checked against our own Haiku runs:

- Always keep: a stop before deleting data, publishing, deploying, or touching secrets that no one confirmed. Step 1 now proceeds on assumptions only for work that can be undone. (Our runs: unattended Haiku on 1.2.0 deleted files 4 of 4 times; with this line it stopped and asked 2 of 2.)
- Bite check as actions: for a bug, write the check first and watch it fail (Step 5); otherwise undo the change in a scratch copy and watch it fail (Step 6). With no runnable check, report "read, not run".
- Step 2 reads the repo's instruction files first (eg. AGENTS.md). Step 5 keeps callers as they are at a shared-point fix and changes generated or vendored files at their source.
- Step 8 follows load-bearing-prompts when installed; otherwise its rules now include the labeled input block, one-clause exclusions, and the exact output shape for machine-read output.
- Round table: fresh agents get read access to the code and none of the earlier guesses; only this session's patches are removed.

Our own:

- Build ladder: before a new dependency, check the job's size — small jobs get new code, hard-to-get-right jobs get a well-maintained dependency named in the report.
- Glossary: new categories are added and listed in the report for the user to keep or cut.
- Step 6's cold reader is named outright: no context, the cheapest model, deep in a long session.
- "When a fix fails twice" opens with an order instead of an explanation, and points to the case card.

### 1.2.0 — 2026-11-17

- New standing section, "When a fix fails twice", for an agent stuck in a loop of fixes that each break something new. It writes a case card with the stakes, sizes the effort to them, then either runs a round table of at least two separate fresh agents whose answers are leads to test, or keeps working in the thread and raises a known note in the report.
- The one-level-up rule now opens that section; Step 2 points to it, and Step 7 carries the known notes.

### 1.1.1 — 2026-11-14

- Step 5 now points to Step 8 for any prompt the change includes, so the prompt rules apply while the prompt is being written, before the check and the report.

### 1.1.0 — 2026-11-14

- Prompt rules restored as Step 8, an optional step that applies only when the change includes a prompt. It is the step meant for each house to make its own.

### 1.0.0 — 2026-11-14

First numbered version.

- Sizing rule moved to the top: every step is sized to the change, not only the plan review.
- "Always keep" moved out of the numbered steps into a standing section near the top, since it holds through every step.
- Accessibility scoped to changes with a user interface.
- Shortcut comments folded into Step 5 (Build), so they are in place before the cold-reader check.
- Prompt-writing rules replaced by a pointer to the load-bearing-prompts skill.
- Steps renumbered from ten to seven; every cross-reference updated.
- Added `license` and `version` to the skill's frontmatter.
