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
