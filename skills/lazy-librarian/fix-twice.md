# When a fix fails twice

Part of the lazy-librarian skill. Read this file at any step, when a second fix on the same problem fails or the symptom moves to a new place.

Read a second failed fix as a sign the root is still unfound: move one level up, starting with the case card below. Bring fresh eyes to the problem, and keep the session going in this thread.

### Write the case card

- Pause fixing. Write what was expected, what happens, and how to make it happen.
- List each attempt with what it did. Keep the card to things you saw happen.
- Write how much the problem matters: what breaks, and for whom, while it stays.
- Size the rest of this section to that — eg. a misaligned icon gets one fix that holds and a known note; lost user data gets the full round table.

### With subagents: the round table

- Summon at least two fresh agents, from different models or tools when available. Give each one the case card and read access to the code, with this job: find the cause and change nothing. Keep earlier guesses about the cause out of what they see.
- Run them separately, so neither sees the other's answer.
- Ask each one to return the cause in one sentence, its evidence, and one prediction: where else the problem must show up, and where it can't.
- Treat every answer as a lead to test. Lay the answers side by side and read their shape: what each one noticed, where they point the same way, and where they split. Test first where they point the same way.
- Test each prediction and follow the one that holds.
- Remove the earlier patches this session made, and keep every other change in the worktree as it is. Then fix once, at the cause.
- When every prediction fails, log what is now ruled out as a known note and keep going on that narrower ground.

### Without subagents: the known note

- Keep working in this thread, without stopping.
- Before the next fix, write the cause in one sentence and test one prediction.
- Log the case card as a known note, marked "diagnosed in-thread, unreviewed", with a clear description of what the error looks like.
- Raise each known note in the report, so the user decides what happens to it.
