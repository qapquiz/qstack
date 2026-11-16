---
name: how
description: "Use for \"how does X work\", code walkthroughs before changing something, and placement / ownership / layering questions (\"where should this live\", \"which package owns this\", \"is this the right layer\"). Explains subsystem architecture, runtime flow, onboarding mental models. Use why for motivation."
disable-model-invocation: true
---

# How

Explore the codebase to answer "how does X work?" questions.

## 1. Explain

If the scope is ambiguous, state your interpretation and explore. The user can redirect.

Spawn one pi process that explores and explains in one pass:

```bash
pi -p --no-session --tools read [--provider <p> --model <m>[:<thinking>]] \
  "<brief>" > /tmp/how-explainer.out 2>&1
```

Give it the user's question and this brief:

> You are writing an architectural explanation for a senior engineer. Explore the codebase and write an explanation that a senior engineer unfamiliar with this area could read and walk away with a solid mental model. They should understand the architecture well enough to start working in it confidently. You have read-only access to the codebase.

`--tools read` keeps the spawn read-only. Run the seat on your own model (pass your own `--provider`/`--model`); `how` has no role line in the `pstack models` section.

## 2. Present

Read the spawn's output and present it. Light edits for clarity or context from the conversation are fine. Do not substantially rewrite it.
