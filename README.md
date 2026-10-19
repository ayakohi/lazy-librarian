# Lazy Librarian

A coding skill for Claude. It builds what the task needs, puts every piece where its concept belongs, and explains plainly what it did.

Decisions are ranked in this order: **correct → well-placed → readable → small.**

## Here's what it does for you

You know the loop. You ask Claude for one thing, it builds you five. A new file here, a helper there, a library you never asked for. Then you spend the next hour auditing what it made instead of using it.

This skill changes how Claude behaves before and after it writes code:

- **It checks first.** Before building, it asks whether the thing needs to exist and points out conflicts or risks in what you asked for.
- **It uses what you already have.** Existing code, built-in tools and installed packages come before writing anything new.
- **It puts things where they belong.** Login stuff goes with login stuff. No random files dumped wherever there's space.
- **It still builds the whole thing.** Nothing you asked for gets skipped, and security, input checks and error handling always stay in.
- **It tells you what it did, plainly.** What it reused, what it created and why, what it skipped, and what could go wrong — with file names, so you know where to look.

Less stuff to audit. And when you do audit, it's easy to find.

## What it does, step by step

1. **Check the frame** — confirm the task needs to exist; list conflicts, assumptions and risks.
2. **Understand the code** — read the files the change touches, trace the data flow, fix bugs at the shared root.
3. **Choose the right place** — by concept, not by free space.
4. **Build what the task needs** — reuse → standard library → native feature → installed dependency → new code.
5. **Always keep** — validation, data-loss handling, security, accessibility, everything explicitly asked for.
6. **Leave a check** — one small test for real logic.
7. **Mark shortcuts** — with `shortcut:` comments naming the limit and upgrade path.
8. **Report back** — explanation first, file paths for everything, risks listed.

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

The build ladder (Step 4), the root-cause rule (Step 2), the always-keep list (Step 5), the one-small-test rule (Step 6) and the shortcut-comment idea (Step 7) are adapted from [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert (MIT). Placement-by-concept (Step 3), the decision ranking, and the reporting style are this skill's own.
