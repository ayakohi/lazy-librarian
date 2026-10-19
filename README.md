# Lazy Librarian

A coding skill for Claude. It builds what the task needs, puts every piece where its concept belongs, and explains plainly what it did.

Decisions are ranked in this order: **correct → well-placed → readable → small.**

## Here's what it does for you

You know the loop. You ask Claude for one thing, it builds you five. A new file here, a helper there, a library you never asked for. Or the opposite: it crams your new feature into some old file because there was room. Then you spend the next hour auditing what it made instead of using it.

This skill changes how Claude behaves before and after it writes code:

- **It checks first.** Before building, it asks whether the thing needs to exist. If your request has a problem, it tells you why and suggests a better way.
- **It reads before it touches.** It looks at the real code, finds the actual root of a bug, and reviews the plan for risks before writing anything.
- **It puts things where they belong.** Login stuff goes with login stuff. A new job gets its own home instead of being squeezed into whatever file had space.
- **It uses what you already have, when it fits.** Existing code, built-in tools and installed packages come first, as long as they belong to the same idea.
- **It builds for later, too.** It adds structure or a library when that makes the code clearer or easier to maintain down the road.
- **It still builds the whole thing.** Nothing you asked for gets skipped, and security, input checks and error handling always stay in.
- **It tells you what it knows vs. what it's guessing.** Checked facts, conclusions and guesses are labeled as such.
- **It tells you what it did, plainly.** What it reused, what it created and why, what it skipped, and what could go wrong — with file names, so you know where to look.

Less mess to audit. And when you do audit, it's easy to find.

## What it does, step by step

1. **Check the frame** — confirm the task needs to exist; push back with a better approach when the request doesn't hold up.
2. **Understand the code** — read the files the change touches, trace the data flow, fix bugs at the shared root, note risks.
3. **Review the plan** — conflicts, assumptions, tradeoffs, complexity, maintainability, failure modes, operational risks.
4. **Choose the right place** — by concept, not by free space. Structure is cheap; confusion is expensive.
5. **Build what the task needs** — the simplest solution that keeps concepts apart: reuse → standard library → native feature → installed dependency → new dependency → new code, skipping anything that belongs to a different concept.
6. **Always keep** — validation, data-loss handling, security, accessibility, everything explicitly asked for.
7. **Verify before stating** — check facts; label the rest as observation, inference or speculation.
8. **Leave a check** — one small test for real logic, using the project's own test setup.
9. **Mark shortcuts** — with `shortcut:` comments naming the limit and upgrade path.
10. **Report back** — plain explanation, file paths for everything, risks listed, one clear next action.
11. **Prompts inside the codebase** — written as plain, positive orders a cheap model can follow cold.

The skill itself is one file: [`skills/lazy-librarian/SKILL.md`](skills/lazy-librarian/SKILL.md).

## Install

**Easiest way, in Claude Code:** download `SKILL.md`, then tell Claude Code:

```
Install this file as a personal skill called lazy-librarian, and add a line to my CLAUDE.md saying to use the lazy-librarian skill on every coding task.
```

**By hand, in Claude Code:** copy `skills/lazy-librarian/SKILL.md` to `~/.claude/skills/lazy-librarian/SKILL.md`. To make it apply on every coding task, add this line to your `CLAUDE.md`:

```
Use the lazy-librarian skill on every coding task.
```

**claude.ai:** add `SKILL.md` as a skill in your settings.

## Credit

The build ladder (Step 5), the root-cause rule (Step 2), the always-keep list (Step 6), the one-small-test rule (Step 8) and the shortcut-comment idea (Step 9) are adapted from [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert (MIT). Placement-by-concept (Step 4), concept boundaries and long-term building (Step 5), verification labels (Step 7), the decision ranking, the pushback rule, the prompt rules and the reporting style are this skill's own.
