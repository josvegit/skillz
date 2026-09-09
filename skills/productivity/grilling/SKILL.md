---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Pick the frontier question that unblocks the most and ask **that one alone**. Never bundle questions.

## Asking a question

Ask with the `AskUserQuestion` tool, one question per call, never as prose. Rules for every call:

- **`multiSelect: true`**, so the user can combine options or pick none and type their own. The harness adds an "Other" free-text option automatically; do not add one yourself.
- **Carry an example.** The `question` text ends with a short concrete example of what an answer looks like for _this_ project (e.g. "Which storage backend? For example: Postgres for orders, Redis for sessions.").
- **2 to 4 options**, each `description` a one-line trade-off. Put your recommended option first with " (Recommended)" appended to its label.
- Keep the `header` chip to a couple of words.

If `AskUserQuestion` is unavailable in this harness, fall back to this format, still one question at a time:

```
❓ **<question title>**: <question body>. Example: <a plausible answer>
   a) <option> (Recommended) b) <option> c) <option> d) Something else

➡️ <why you recommend a>
```

## Between questions

Each answer reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next question.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
