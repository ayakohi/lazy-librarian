---
name: load-bearing-prompts
description: How to write prompts for zero-context executors — single API calls, cron and distillation jobs, cold threads, subagent one-shots, prompt strings embedded in code. Use this skill whenever writing, revising, or reviewing any prompt that a model will run with no conversation history in a single pass, especially a cheap or fast model. Trigger even when the user just says "write the prompt for this call" or "fix this prompt", and whenever code contains a string that gets sent to a model API.
license: MIT
metadata:
  version: "1.0.0"
  authors: ayakohi
---

# Load-Bearing Prompts

Two readers exist here. **You**, a flagship model reading this skill — contrast examples below are for you, and only the ✓ shapes may appear in your output. **The executor**, reading the prompt you write — a model with zero context, making one fast pass, often cheap, complying rather than reasoning. Every sentence you hand it must be an order it can execute immediately.

Write the prompt as a work order: orders only, each one the executor can carry out on its first read.

## Rules

Every example below comes from one scenario: a cron job where a cheap model turns a week of raw customer feedback tickets into a digest posted in the product team channel.

**Open with placement.** State the executor's role, its input, and where the output lands. Put the input data in its own labeled block after the orders, and name that block in the placement line.
- ✗ "This digest matters because the whole team depends on it."
- ✓ "You turn this week's customer feedback tickets, given in the <tickets> block below, into a digest posted in the product team channel."

**Set vantage and pronouns directly.**
- ✗ "Write from the team's perspective, never as a detached outside observer."
- ✓ "Write in third person plural, use 'users' — eg. 'Users hit failures in CSV export.'"

**State disposition when the output has a voice.** One plain line.
- ✓ "Tone is declarative present tense."

**Instruct, never explain.** Start sentences with verbs. Commentary about why the task matters, or what the distinction "really" is, spends the executor's one pass on nothing.
- ✗ "That distinction is the whole job. A ticket records a complaint; this digest records the state those complaints reveal."
- ✓ "Write the current state of each problem and include the count."

**Frame positively; exclusions get one action-verb clause.**
- ✗ "This is not a changelog and not a list of every ticket."
- ✓ "Discard praise-only tickets, duplicates, and requests already shipped."

**Fix upstream.** When a draft output shows an unwanted trait, rewrite the instruction that produced it. A ban names the trait into the prompt and invites it — the pink elephant in the room: telling a one-pass executor "don't think of X" places X in its context, and compliance-mode models echo what is present. Exception: exact-string bans are mechanically checkable and safe.
- ✗ "Avoid poetic compression."
- ✓ (rewrite the source instruction) "Use simple words and short sentences."
- ✓ (string ban, allowed) "Never output the word 'delve'."

**Treat the reader as cold.** The executor knows nothing: no project history, no earlier outputs, no shared vocabulary. It is a failure mode if any sentence requires parsing a history or an existing example the reader has never seen. Put everything the sentence needs inside the sentence.
- ✗ "Format entries like last week's digest."
- ✓ "Format each entry as: feature name, ticket count in parentheses, one-line current state — eg. 'Export (41 tickets): CSV downloads fail on files over 10MB.'"

**One exemplar per instruction, to-be-followed only.** Inline, marked eg., showing the target shape. In the prompt you write, cite only examples to be followed — a wrong-version quoted for contrast prints the exact output you paid to prevent.
- ✗ "'Users reported issues with export' is wrong; 'Export (41 tickets): CSV downloads fail on files over 10MB' is right."
- ✓ "eg. 'Export (41 tickets): CSV downloads fail on files over 10MB.'"

**Give the exact shape when a program reads the output.** Show the full output once as an eg., and make it the last order in the prompt.
- ✓ (if a dashboard read the digest instead) "Reply with only one JSON array — eg. [{"feature": "Export", "tickets": 41, "state": "CSV downloads fail on files over 10MB"}]"

**Use simple words and short sentences.** Write at the level of the cheapest model that will ever run this prompt.

## Worked example

The full before and after for the same digest call.

**Before (fog — for your eyes only, never write this):**
> You are the listener at the door of the product. Each week, hundreds of voices arrive — some angry, some grateful, most tired. Your task is not to count them; it is to hear what they are, together, trying to say. A digest is not a pile of tickets — it is the weather those tickets make. Do not be robotic. Do not drown the reader in corporate jargon — no "pain points," no "actionable insights," no "learnings." Never editorialize, never speculate, never pad. "Users reported issues with export" is dead on arrival; "Export is where trust goes to die this week" is alive. Remember who reads this: engineers at 9am, coffee in hand. Brevity is mercy. Above all, resist the urge to summarize everything — curation is the soul of the digest.

The crimes, itemized:
- Opens with an identity poem. The executor cannot act on being "the listener at the door."
- Three not-X-but-Y pairs, each paying double tokens to give one order.
- Bans that print the banned thing: the jargon list places "pain points" and "learnings" directly into the executor's context.
- A wrong/right quote pair that ships the dead sentence into the prompt.
- "Resist the urge" names the urge.
- Closes on rhetoric ("curation is the soul of the digest") — a one-pass executor either ignores it or imitates it. Both are losses.

**After (under this skill):**
> You turn this week's customer feedback tickets, given in the <tickets> block below, into a digest posted in the product team channel. Write in third person plural, use "users" — eg. "Users hit failures in CSV export." Tone is plain declarative present tense. Group tickets by feature, one line per feature, biggest ticket count first — eg. "Export (41 tickets): CSV downloads fail on files over 10MB." Write the current state of each problem and include the count. Keep the digest under 120 words. Discard praise-only tickets, duplicates, and requests already shipped. End after the last feature line.
>
> <tickets>
> {this week's tickets}
> </tickets>

Every sentence is an order the executor can act on immediately. Placement first, naming the labeled input block. Vantage and pronouns explicit, with an exemplar. Disposition in one plain line. Format orders each carry their own eg. One exclusion, one action verb, then stop. A hard endpoint so the output cannot ramble a sign-off. The data sits last, in its labeled block. When this prompt misfires, the faulty order is easy to find and tighten.

## The test

Reread the finished prompt one sentence at a time. Ask of each: can a model that knows nothing act on this sentence right now, with only what the sentence itself provides? Cut every sentence that fails. Then scan once more for any trait named in order to be avoided; flip it into a positive instruction or cut it. Confirm every quoted example is one to be followed.
