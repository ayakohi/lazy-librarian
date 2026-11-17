---
name: lazy-librarian
description: Use on every coding task — writing, editing, fixing, debugging, refactoring, reviewing, planning or designing code, choosing files, folders, dependencies or libraries, writing scripts, configs, tests, or prompts inside a codebase. Decides what to build, where it belongs, how to check it, and how to report it.
license: MIT
metadata:
  version: "1.2.0"
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
- Write any prompt the change includes by the rules in Step 8.
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
- Carry forward what earlier steps sent here: the assumptions from Step 1, the new names from Step 4, the tests named in Step 6, the shortcuts from Step 5, and each known note from "When a fix fails twice" for the user to decide on.
- State coverage exactly as done, for each claim: run, read in full, spot-checked, reasoned without running, or untested.
- Give every defect you noticed a disposition, including ones outside the task: fixed, listed for later with its file path, or accepted with the reason.
- End with a one-line takeaway and one concrete next action.

## Step 8 — Prompts inside the codebase

Apply this step when the change includes a prompt that a model will run.

- Open with placement: the model's role, its input, and where its output lands.
- Set vantage and disposition in one line each when the output has a voice — eg. "Write in second person. Tone is plain and warm."
- Write every sentence as an order that starts with a verb. Leave the reasons out.
- Frame every instruction positively to avoid pink elephant instances: describe the target, and leave unwanted traits unnamed. When an output shows an unwanted trait, rewrite the instruction that produced it.
- Write for the cheapest model that will ever run the prompt: simple words, short sentences, and everything each sentence needs inside that sentence.
- Give each instruction at most one inline example, marked eg., showing only the target shape.
- Keep each sentence that changes what the model does. Cut each sentence the model already follows without it.
- When a load-bearing-prompts skill is installed, use it for the full method.

## When a fix fails twice

Apply this section at any step, when a second fix on the same problem fails or the symptom moves to a new place. Repeated fixes on one problem mean the fixes are aimed at the wrong root: move one level up and find it. Bring fresh eyes to the problem, and keep the session going in this thread.

### Write the case card

- Pause fixing. Write what was expected, what happens, and how to make it happen.
- List each attempt with what it did. Keep the card to things you saw happen.
- Write how much the problem matters: what breaks, and for whom, while it stays.
- Size the rest of this section to that — eg. a misaligned icon gets one fix that holds and a known note; lost user data gets the full round table.

### With subagents: the round table

- Summon at least two fresh agents, from different models or tools when available. Give each one only the case card, with this job: find the cause and change nothing.
- Run them separately, so neither sees the other's answer.
- Ask each one to return the cause in one sentence, its evidence, and one prediction: where else the problem must show up, and where it can't.
- Treat every answer as a lead to test. Lay the answers side by side and read their shape: what each one noticed, where they point the same way, and where they split. Test first where they point the same way.
- Test each prediction and follow the one that holds.
- Remove the earlier patches, then fix once, at the cause.
- When every prediction fails, log what is now ruled out as a known note and keep going on that narrower ground.

### Without subagents: the known note

- Keep working in this thread, without stopping.
- Before the next fix, write the cause in one sentence and test one prediction.
- Log the case card as a known note, marked "diagnosed in-thread, unreviewed", with a clear description of what the error looks like.
- Raise each known note in the report, so the user decides what happens to it.
