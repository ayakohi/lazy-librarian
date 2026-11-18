# Lazy Librarian

*By ayakohi and Cygnus.* · MIT

Two skills for Claude, shipped together:

| Skill | Version | What it's for |
|---|---|---|
| [**lazy-librarian**](skills/lazy-librarian/SKILL.md) | 1.4.0 | Every coding task: what to build, where it goes, how to check it, how to report it. |
| [**load-bearing-prompts**](skills/load-bearing-prompts/SKILL.md) | 1.0.0 | Writing prompts that a model runs cold, in one pass: API calls, cron jobs, subagent one-shots. |

They work on their own. Together, lazy-librarian's Step 8 hands prompt writing to load-bearing-prompts for the full method.

---

## lazy-librarian

A coding skill. It builds what the task needs, puts every piece where its concept belongs, and explains plainly what it did.

Decisions are ranked in this order: **correct → well-placed → readable → small.** Every step is sized to the change, so a one-line fix gets a one-line pass.

Some things hold through every step: input validation, error handling that prevents data loss, security, accessibility basics when there's a user interface, and everything you explicitly asked for.

## Here's what it does for you

You know the loop. You ask Claude for one thing, it builds you five. A new file here, a helper there, a library you never asked for. Or the opposite: it crams your new feature into some old file because there was room. Then you spend the next hour auditing what it made instead of using it.

This skill changes how Claude behaves before and after it writes code:

- **It checks first.** Before building, it asks whether the thing needs to exist. If your request has a problem, it tells you why and suggests a better way.
- **It reads before it touches.** It looks at the real code, finds the actual root of a bug, and reviews the plan for risks before writing anything.
- **It puts things where they belong.** Socks go in the laundry basket, not on the kitchen counter just because there's more space there. Login stuff goes with login stuff, and a new job gets its own home, named after what it does.
- **It uses what you already have, when it fits.** Existing code, built-in tools and installed packages come first, as long as they belong to the same idea.
- **It builds at your scale.** Small fix or big system, it builds what the task calls for. It just keeps everything sorted as it grows.
- **It's upfront about guesses.** When something's unclear, it states its assumption and says what it could and couldn't check.
- **It never cuts the important stuff.** Nothing you asked for gets skipped, and security, input checks and error handling always stay in. Before a destructive change it checks whether it can be undone: if yes, it goes ahead and tells you how to get it back; if not, it stops and asks. Anything it installs on your machine gets listed with how to remove it.
- **It tells you what it did, plainly.** What it reused, what it created and why, what it skipped, and what could go wrong — with file names, so you know where to look.

Less mess to audit. And when you do audit, it's easy to find.

## What it does, step by step

1. **Check the frame** — confirm the task needs to exist; push back with a better approach when the request doesn't hold up.
2. **Understand the code** — read the files the change touches, trace the data flow, follow every connected chain to a live end, fix bugs at the shared root (and move one level up when a fix keeps turning out differently), note risks.
3. **Review the plan** — conflicts, assumptions, tradeoffs, complexity, maintainability, failure modes, operational risks.
4. **Choose the right place** — by category, not by free space, even for a single item, with names that say why each piece exists and a project glossary to keep them.
5. **Build what the task needs** — at the scale it calls for, with concepts kept apart: reuse → standard library → native feature → installed dependency → new dependency → new code, skipping anything that belongs to a different concept. Deliberate shortcuts get a `shortcut:` comment naming the limit and upgrade path.
6. **Leave a check** — one small test for real logic, using the project's own test setup, seen failing without the change and passing with it; "read, not run" when nothing can run; then one read as the weakest reader: no context, the cheapest model, deep in a long session.
7. **Report back** — plain explanation, file paths for everything, risks listed, exactly what was run versus only read or reasoned, a decision on every defect noticed, one clear next action.

8. **Prompts inside the codebase** *(optional)* — only when the change includes a prompt: plain, positive orders a cheap model can follow cold, keeping only the sentences that change what the model does. This step is the one most worth making your own: swap in your house's prompt rules if you have them.

**When a fix fails twice** — a standing section for when an agent gets stuck, where each fix breaks something new. It pauses to write a *case card* (what was expected, what happens, every attempt, and how much it matters), then sizes the effort to the stakes. With subagents, at least two fresh agents diagnose the case separately; their answers are leads to test, read for their shape, never taken as the truth. Without subagents, it keeps working in the same thread and raises a *known note* in the report for you to decide on. Fresh eyes on the problem, and the session keeps its continuity.

---

## load-bearing-prompts

*By ayakohi.*

A skill for writing prompts that a model runs with no history, in a single pass. That model is often a cheap one that follows orders rather than reasoning. Think of a nightly job that summarizes logs, or a string in your code that gets sent to an API.

It turns a prompt into a work order:

- **Placement first** — the model's role, its input, and where the output lands. The input data goes in its own labeled block, so the model reads it as material.
- **Orders, not explanations** — every sentence starts with a verb and can be acted on at once.
- **Positive framing** — describe what you want; a ban names the unwanted thing and plants it in the model's context.
- **Cold reader** — everything a sentence needs is inside that sentence.
- **One example per order** — showing only the shape to copy.
- **Exact shape for machine-read output** — when a program reads the result, show the full output once.
- **Simple words, short sentences** — written for the cheapest model that will ever run it.

It ends with a worked before-and-after and a sentence-by-sentence test.


---

## Install

Each skill is one folder under `skills/`. load-bearing-prompts is a single `SKILL.md`. lazy-librarian is a `SKILL.md` plus two files it reads only when needed: `prompts.md` (Step 8, when no load-bearing-prompts skill is installed) and `fix-twice.md` (when a fix fails twice). Keep each folder whole. Install one or both.

**Easiest way, in Claude Code:** download the skill's folder, then tell Claude Code, eg.:

```
Install this folder as a personal skill called lazy-librarian, and add a line to my CLAUDE.md saying to use the lazy-librarian skill on every coding task.
```

**By hand, in Claude Code:** copy each folder to `~/.claude/skills/`, so you end up with `~/.claude/skills/lazy-librarian/` (all three files) and `~/.claude/skills/load-bearing-prompts/SKILL.md`. To make lazy-librarian apply on every coding task, add this line to your `CLAUDE.md`:

```
Use the lazy-librarian skill on every coding task.
```

load-bearing-prompts needs no line; it triggers on its own whenever a prompt is being written.

**claude.ai:** zip the skill's folder (eg. `load-bearing-prompts/` with its `SKILL.md` inside) and upload it in claude.ai's skill settings.

## Credit

**lazy-librarian** is written by **ayakohi** and **Cygnus**. **load-bearing-prompts** is written by **ayakohi**, an original method rather than an adaptation.

The build ladder (Step 5), the root-cause rule (Step 2), the always-keep list, the one-small-test rule (Step 6) and the shortcut-comment idea (Step 5) are adapted from [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert (MIT). The reach list (Step 2) and the carry-the-error-handling rule (Step 5) also come from Ponytail. The project glossary (Step 4) and the sentence-pruning test (Step 8) are adapted from [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock (MIT). The traceable-lines rule (Step 5) is adapted from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) (MIT). Félix, Daxi's companion from the House of Solance Discord, benchmarked 1.3.0 across house setups and shaped 1.4.0: installs recorded with how to remove them, a check that fails for the reason the fix removes, naming the owner of a reused live thing, and naming where an unattended job's failures land.

Suggestions shipped in 1.3.0 come from **Anne** and **Chadrien** of [House of Solance](https://github.com/SolanceLab), who stress-tested 1.2.0 across several models and filed them in [issue #1](https://github.com/ayakohi/lazy-librarian/issues/1): the stop before irreversible steps, the action-style bite check, reading the repo's instruction files first, keeping callers as they are at a shared-point fix, changing generated files at their source, the Step 8 rules synced with load-bearing-prompts, and the round table's read access and session-only patch removal.

Placement-by-category and naming (Step 4), concept boundaries (Step 5), live-ended chains and pointers (Steps 2 and 5), the cold-reader check (Step 6), the one-level-up rule and the fix-fails-twice section with its case card, round table and known note, the decision ranking, the sizing rule, the pushback rule, the prompt rules, the coverage report and the reporting style are lazy-librarian's own.

## Changes

See [`CHANGELOG.md`](CHANGELOG.md).

## License

MIT — see [`LICENSE`](LICENSE). It also carries Ponytail's MIT notice for the parts of lazy-librarian adapted from it.
