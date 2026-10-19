# Set up pstack

In this page you install the plugin, pick which models pstack uses, and run your first task. Setup is one command plus a short conversation.

## Install the plugin

From a checkout of this repository, run:

```bash
pi install /path/to/qstack        # local path
# or, once published:
pi install git:github.com/<you>/qstack
```

`pi list` confirms the package is installed. Project-local alternative: the repo already carries the skills under `skills/`, so running pi inside a project that vendors them needs no install at all — just approve the project-local files when prompted (or start once with `pi --approve`).

## Pick your models

Run:

```text
/skill:setup-pstack
```

[`/skill:setup-pstack`](../../skills/setup-pstack/SKILL.md) runs `pi --list-models` to detect the models you have access to, asks for a reasoning budget, shows you each role (code delegates, judgment, the review panels), and asks what you want. Answer the questions. It writes the `pstack models` section of `~/.pi/agent/AGENTS.md`, a small always-loaded config every pstack skill reads.

The budget you pick in `/skill:setup-pstack` sets the `:<thinking>` suffix of every configured model. `unlimited` lifts each entry to `max`; `medium` and `small` lower it to `high` or `medium`. `large` matches the skill defaults.

You only override what you care about. A role with no line in the section keeps the skill's default: the seat runs on your own model. To restore that default, delete the role's line. A rerun of `/skill:setup-pstack` keeps any role whose model you changed. When the skill defaults change, a section written before the change still pins the old choices, so delete those role lines, or delete the section, then run `/skill:setup-pstack` again.

Set a role to `inherit-parent` or `auto` and pstack spawns the seat on your own `--provider`/`--model`, so it runs on the same model as this session. Both values mean the same thing, and neither is a model entry. For a panel role the value is a list, and one subagent runs per entry, so the list length sets the panel size. Setup also configures `swarm workers`, the default model for every `/skill:swarm` worker unless a race names a model for each arm.

## Accept the verification offer, or don't

At the end of setup, `/skill:setup-pstack` looks for a way to prove app behavior in your project, either a `verify-*` skill or an existing harness. If it finds neither, it offers once to generate one with [`/skill:create-verification-skill`](../../skills/create-verification-skill/SKILL.md).

Say yes and it writes `.agents/skills/verify-<app>/`, a project-local skill that teaches agents to drive your app the way a user does. It proves the skill works once before handing it over. Say no and setup moves on. You can run `/skill:create-verification-skill` yourself any time. [Verify and ship](./06-verify-and-ship.md#create-a-project-verification-skill) covers it in depth.

If you're new to pstack, say yes. An agent that can check its own work keeps going until the check passes. An agent that can't hands every result back to you to check by hand. Of everything in this guide, the verification skill pays off the most.

Then setup offers once to generate a formal verification skill with [`/skill:create-formal-verification-skill`](../../skills/create-formal-verification-skill/SKILL.md), unless the project already has one. Tests check the cases someone thought to write. A formal verification skill checks one key property of your code, such as "a job never runs twice at once", in every case within a stated scope. Say yes when your code has concurrency, retries, a state machine, or a parser, where a bug is costly and tests rarely reach the case that breaks. Otherwise say no, and run it later if that changes. [Verify and ship](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill) covers it.

After setup, new spawns pick the section up immediately. Restarts are not required.

## Keep the cost in check

pstack spends extra tokens on subagents and review panels. That's the price of the rigor. To spend fewer:

- Rerun `/skill:setup-pstack` and pick a smaller reasoning budget or cheaper models. A strong model in the main chat with cheaper, faster models in the code roles is a good split.
- Set a role to `auto` or `inherit-parent` so it runs on the chat's own model.
- Shorten a panel list. Each entry runs one subagent.
- Save `/skill:poteto-mode` for work that needs rigor. A small, obvious edit doesn't.

## Run your first task

Pick something real but small, and describe it the way you'd describe it to a colleague:

```text
/skill:poteto-mode add a --json flag to this command. text output stays byte-identical. verify both.
```

Watch the todo list. Its first items are the matched playbook's steps copied in, the Feature playbook for this prompt. If `/skill:poteto-mode` skips a step, the step stays in the list with `skip: <reason>`, so you can see what it chose not to do.

From here you can type normal follow-ups. `/skill:poteto-mode` is sticky. It stays on for the conversation until you opt out by saying so.

Next: [Route work through `/skill:poteto-mode`](./02-poteto-mode.md).
