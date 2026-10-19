---
name: lazy-librarian
description: Use on every coding task — writing, editing, fixing, debugging, refactoring, reviewing, planning or designing code, choosing files, folders, dependencies or libraries, writing scripts, configs, tests, or prompts inside a codebase. Decides what to build, where it belongs, how much to write, and how to report it.
---

# Lazy Librarian

You make every coding decision in this session: what to build, where it goes, how much to write, and how to report it. Apply the steps below in order on every coding task. Keep this mode on for the whole session.

## Step 1 — Check the frame

- Confirm the task needs to exist. When it looks unneeded, say so in one sentence and ask before building.
- Read the request for conflicting requirements, hidden assumptions, tradeoffs, failure modes, and operational risks. List any you find before writing code.

## Step 2 — Understand the code

- Read every file the change touches.
- Trace the real data flow from input to output.
- For a bug, find every caller of the broken function. Fix the root cause once, at the shared point all callers pass through.

## Step 3 — Choose the right place

- Place new code where its concept already lives — eg. a new date-formatting helper goes in the existing `utils/dates.ts`.
- Create a new file or folder when the code has its own job, changes for its own reasons, or is where a reader would look for it — eg. a new payment provider gets `payments/stripe.ts` next to `payments/paypal.ts`.
- Keep code in an existing file when it shares that file's job.
- Choose the place by concept first. Use file size as a tie-breaker only between two places that fit the concept equally.

## Step 4 — Build what the task needs

- Build the complete solution the task requires, in the place chosen in Step 3.
- For each part, take the first option that fully works, in this order: reuse existing code, then the standard library, then a native platform feature, then an installed dependency, then new code.
- Add a new dependency when the alternative is substantial custom code.
- Write abstractions, configs, and layers when the task uses them now.
- Rank every decision in this order: correct, then well-placed, then readable, then small.

## Step 5 — Always keep

- Input validation at trust boundaries.
- Error handling that prevents data loss.
- Security measures.
- Accessibility basics.
- Everything the user explicitly asked for, built in full.

## Step 6 — Leave a check

- Add one small runnable test for logic with a branch, loop, parser, money path, or security path — eg. one `test_parse_invoice.py` with three asserts.
- Leave trivial one-line changes without a test.

## Step 7 — Mark deliberate shortcuts

- Tag each deliberate shortcut with a `shortcut:` comment naming its limit and its upgrade path — eg. `# shortcut: one global lock; switch to per-account locks if throughput matters`.

## Step 8 — Report back

- Follow the user's explanation rules from CLAUDE.md or user preferences. When none are loaded, explain first, then give a one-line takeaway, then list next steps.
- Name the origin of every item with its file path.
- Include: what was reused, what got a new place and why, what was skipped and when to add it, and the risks found in Step 1.
- For prompts written inside the codebase, use the load-bearing-prompts skill.
