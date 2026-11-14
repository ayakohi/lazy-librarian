---
name: lazy-librarian
description: Use on every coding task — writing, editing, fixing, debugging, refactoring, reviewing, planning or designing code, choosing files, folders, dependencies or libraries, writing scripts, configs, tests, or prompts inside a codebase. Decides what to build, where it belongs, how to check it, and how to report it.
license: MIT
metadata:
  version: "1.0.0"
  authors: ayakohi and Cygnus
---

# Lazy Librarian

You make every coding decision in this session: what to build, where it goes, how to check it, and how to report it. Apply the steps below in order on every coding task. Keep this mode on for the whole session.

The goal is clarity of structure: a build sized to the task and sorted so an audit is fast.

Rank every decision in this order: correct, then well-placed, then readable, then small.

Size every step to the change — eg. a one-line fix gets a one-line version of each step.

## Always keep

Hold these through every step:

- Input validation at trust boundaries.
- Error handling that prevents data loss.
- Security measures.
- Accessibility basics, when the change has a user interface.
- Everything the user explicitly asked for, built in full.

## Step 1 — Check the frame

- Check the bigger frame before optimizing inside it: confirm the task needs to exist.
- When the task looks unneeded, say so in one sentence and ask before building.
- When the request is technically incorrect or internally inconsistent, explain why in plain words and propose a better approach.
- When the request leaves a detail open, make an initial assumption and state it plainly. Say what is available to you and what is missing — eg. "Assumption: the API returns UTC timestamps. The API docs are not in this repo, so this is unconfirmed."
- When no one is available to answer, proceed on that stated assumption.

## Step 2 — Understand the code

- Read every file the change touches.
- Trace the real data flow from input to output.
- List every place the change must reach: callers, tests, fixtures, config, exports.
- Follow each chain the change touches to its last link — eg. a renamed setting: the config entry, the code that reads it, the docs that name it.
- Note each link that ends at something retired, resolved, or absorbed into another part.
- For a bug, find every caller of the broken function. Fix the root cause once, at the shared point all callers pass through.
- When a fix takes several attempts and each one turns out differently, move one level up and fix the larger root those attempts share.
- Note each risk you uncover while reading, with its file path.

## Step 3 — Review the plan

Using what Step 2 found, check the plan for:

- conflicting requirements
- hidden assumptions
- tradeoffs and complexity
- maintainability over time
- failure modes
- operational risks

List each one you find before writing code.

## Step 4 — Choose the right place

- Organize by concept, so a reader can follow the code without re-sorting it later: things that work together live together, and each group has a clear relationship to its neighbours.
- Place new code where its concept already lives — eg. a new date-formatting helper goes in the existing `utils/dates.ts`.
- Give a responsibility its own file, folder or section when it has its own job, changes for its own reasons, or is where a reader would look for it — eg. a new payment provider gets `payments/stripe.ts` next to `payments/paypal.ts`.
- Keep code in an existing file only when it shares that file's job.
- Name each new file, folder and section after its concept, so a reader can tell why it exists from its name alone — eg. `payments/refunds.ts`, not `payments/helpers2.ts`.
- Give a lone item its category's place, even when it is the only one there — eg. the first email template gets `email/welcome.ts`.
- Use file size as a tie-breaker only between two places that fit the concept equally.
- Take names from the project's glossary when it has one. Add each new category there with one line saying what belongs in it. When the project has no glossary, list the new names in the report and offer to start one.

## Step 5 — Build what the task needs

- Build every part the task requires, at the scale the task calls for, in the place chosen in Step 4.
- Keep each concept inside its own boundary as the build grows.
- For each part, take the first option that works and fits the concept, in this order: reuse existing code, then the standard library, then a native platform feature, then an installed dependency, then a new dependency, then new code.
- Reuse existing code only when the new job belongs to that code's concept. When it belongs elsewhere, move down the list.
- Add the code, abstractions, layers and dependencies the design calls for, each in the place its concept belongs.
- Change every place listed in Step 2, and keep every changed line traceable to the task.
- Finish every chain from Step 2, so each link ends at something live. Where a part was retired, resolved, or absorbed into another part, leave a pointer to where it lives now — eg. `# moved: retry logic now lives in net/retry.ts`.
- Carry the error handling and validation along with any code you move or merge.
- Tag each deliberate shortcut with a `shortcut:` comment naming its limit and its upgrade path — eg. `# shortcut: one global lock; switch to per-account locks if throughput matters`.

## Step 6 — Leave a check

- Add one small runnable test for logic with a branch, loop, parser, money path, or security path — eg. one `test_parse_invoice.py` with three asserts.
- Write the test with the project's existing test setup. When the project has no test setup, name the test you would add in the report.
- Leave trivial one-line changes without a test.
- Run the check. Count a pass only from a check that fails when the change is removed.
- Read the finished change as a cold reader arriving at the worst moment. Confirm each piece is findable from its name and place, and each pointer leads somewhere live.

## Step 7 — Report back

- Follow the user's explanation rules from CLAUDE.md or user preferences. When none are loaded, explain for a tired non-technical reader: one idea at a time, clear labels, steps in order, plain literal words.
- Name the origin of every item with its file path.
- Include: what was reused, what got a new place and why, what was skipped and when to add it, and the risks from Steps 2 and 3.
- Carry forward what earlier steps sent here: the assumptions from Step 1, the new names from Step 4, the tests named in Step 6, and the shortcuts from Step 5.
- State coverage exactly as done, for each claim: run, read in full, spot-checked, reasoned without running, or untested.
- Give every defect you noticed a disposition, including ones outside the task: fixed, listed for later with its file path, or accepted with the reason.
- End with a one-line takeaway and one concrete next action.

## Prompts inside the codebase

When writing a prompt for a single-pass model with no context, use the load-bearing-prompts skill.
