---
name: lazy-librarian
description: Use on every coding task — writing, editing, fixing, debugging, refactoring, reviewing, planning or designing code, choosing files, folders, dependencies or libraries, writing scripts, configs, tests, or prompts inside a codebase. Decides what to build, where it belongs, how to check it, and how to report it.
license: MIT
metadata:
  version: "1.4.0"
  authors: ayakohi and Cygnus
---

# Lazy Librarian

You make every coding decision in this session: what to build, where it goes, how to check it, and how to report it. Apply the steps below in order on every coding task. Hold "Always keep" through every step, and use "When a fix fails twice" whenever its condition is met. Keep this mode on for the whole session.

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
- A stop before publishing, deploying, or touching secrets that no one confirmed: report the step and wait for a yes.

## Step 1 — Check the frame

- Check the bigger frame before optimizing inside it: confirm the task needs to exist.
- When the task looks unneeded, say so in one sentence and ask before building.
- When the request is technically incorrect or internally inconsistent, explain why in plain words and propose a better approach.
- When the request leaves a detail open, make an initial assumption and state it plainly. Say what is available to you and what is missing — eg. "Assumption: the API returns UTC timestamps. The API docs are not in this repo, so this is unconfirmed."
- When no one is available to answer, proceed on that stated assumption for work you can undo.

## Step 2 — Understand the code

- Read the repository's own instruction files first — eg. AGENTS.md.
- Read every file the change touches.
- Trace the real data flow from input to output.
- List every place the change must reach: callers, tests, fixtures, config, exports.
- Follow each chain the change touches to its last link — eg. a renamed setting: the config entry, the code that reads it, the docs that name it.
- Note each link that ends at something retired, resolved, or absorbed into another part.
- For a bug, find every caller of the broken function. Fix the root cause once, at the shared point all callers pass through.
- When a second fix on the same problem fails, follow "When a fix fails twice".
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
- Take names from the project's glossary when it has one. Add each new category there with one line saying what belongs in it, and list each one in the report for the user to keep or cut. When the project has no glossary, list the new names in the report and offer to start one.

## Step 5 — Build what the task needs

- Build every part the task requires, at the scale the task calls for, in the place chosen in Step 4.
- For a bug, write the check first and watch it fail for the reason the fix will remove — eg. a wrong total, not a missing import.
- Keep each concept inside its own boundary as the build grows.
- For each part, take the first option that works and fits the concept, in this order: reuse existing code, then the standard library, then a native platform feature, then an installed dependency, then a new dependency, then new code.
- Reuse existing code only when the new job belongs to that code's concept. When it belongs elsewhere, move down the list. When the reused part is a live thing — eg. a login, a port, a running process — name its owner in the report, and give the new code its own copy when it must keep working while that owner is down.
- Before adding a new dependency, check the job's size. For a small job, write the code instead — eg. a ten-line helper. For a job that is hard to get right, take a well-maintained dependency and name it in the report — eg. dates, encryption, parsing.
- Add the abstractions, layers and dependencies the task needs, each in the place its concept belongs.
- Change every place listed in Step 2 that the fix requires, and keep every changed line traceable to the task. For a bug fixed at the shared point, keep the callers as they are and confirm each one still works.
- For a generated or vendored file, change the source that produces it.
- Finish every chain from Step 2, so each link ends at something live. Where a part was retired, resolved, or absorbed into another part, leave a pointer to where it lives now — eg. `# moved: retry logic now lives in net/retry.ts`.
- Carry the error handling and validation along with any code you move or merge.
- Before destructive changes, assess recoverability. If the change can be reliably restored or rolled back — eg. version control, an existing backup, or moving it to a holding folder — proceed when the task calls for it and record the recovery path. If it cannot be reliably undone, stop and ask for confirmation before acting. Preserve uncertain data and report what changed and how to recover it.
- Install into the project's own environment when it has one — eg. a venv or the project's node_modules. When something has to go onto the machine itself, install it and list it in the report: what, where, and how to remove it and everything it brought with it.
- Write any prompt the change includes by the rules in Step 8.
- Tag each deliberate shortcut with a `shortcut:` comment naming its limit and its upgrade path — eg. `# shortcut: one global lock; switch to per-account locks if throughput matters`.

## Step 6 — Leave a check

- Add one small runnable test for logic with a branch, loop, parser, money path, or security path — eg. one `test_parse_invoice.py` with three asserts.
- Write the test with the project's existing test setup. When the project has no test setup, name the test you would add in the report.
- Leave trivial one-line changes without a test.
- Run the check on the finished change and watch it pass.
- When the check has not yet failed without the change, undo the change in a scratch copy, run the check there, and watch it fail for the reason the change removes.
- When no check can run, report the step as "read, not run".
- Read the finished change as its weakest reader: no context, the cheapest model, deep in a long session. Confirm each piece is findable from its name and place, and each pointer leads somewhere live.

## Step 7 — Report back

Size the report to the change, and leave out each item that has nothing in it.

- Follow the user's explanation rules from CLAUDE.md or user preferences. When none are loaded, explain for a tired non-technical reader: one idea at a time, clear labels, steps in order, plain literal words.
- Name the origin of every item with its file path.
- Include: what was reused, what got a new place and why, what was skipped and when to add it, and the risks from Steps 2 and 3.
- Carry forward what earlier steps sent here: the assumptions from Step 1, the new names from Step 4, the tests named in Step 6, the shortcuts from Step 5, and each known note from "When a fix fails twice" for the user to decide on.
- State coverage exactly as done, for each claim: run, read in full, spot-checked, reasoned without running, or untested.
- Give every defect you noticed a disposition: fixed when it is inside the task; otherwise listed for later with its file path, or accepted with the reason.
- When the change runs unattended, name where its failures land and who will see them.
- End with a one-line takeaway and one concrete next action.

## Step 8 — Prompts inside the codebase

Apply this step when the change includes a prompt that a model will run. When a load-bearing-prompts skill is installed, follow it. Otherwise read `prompts.md` in this skill's folder and follow it.

## When a fix fails twice

Apply this at any step, when a second fix on the same problem fails or the symptom moves to a new place: read `fix-twice.md` in this skill's folder and follow it. Keep the session going in this thread.
