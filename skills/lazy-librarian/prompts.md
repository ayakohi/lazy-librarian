# Step 8 rules — prompts inside the codebase

Part of the lazy-librarian skill. Read this file when the change includes a prompt that a model will run and no load-bearing-prompts skill is installed.

- Open with placement: the model's role, its input, and where its output lands.
- Put the input data in its own labeled block after the orders, and name the block in the first line — eg. "Classify the email in the <email> block below."
- Set vantage and disposition in one line each when the output has a voice — eg. "Write in second person. Tone is plain and warm."
- Write every sentence as an order that starts with a verb. Leave the reasons out.
- Frame every instruction positively to avoid pink elephant instances: describe the target, and leave unwanted traits unnamed. When the output must exclude something exact, state it in one clause that starts with a verb. When an output shows an unwanted trait, rewrite the instruction that produced it.
- Write for the cheapest model that will ever run the prompt: simple words, short sentences, and everything each sentence needs inside that sentence.
- Give each instruction at most one inline example, marked eg., showing only the target shape.
- Keep each sentence that changes what the model does. Cut each sentence the model already follows without it.
- When a program reads the output, end with the exact output shape — eg. `Reply with only: {"category": "billing", "urgent": false}`
