# Set up pstack

In this page you install the plugin, pick which models pstack uses, and run your first task. Setup is one command plus a short conversation.

## Install the plugin

From a checkout of this repository, run:

```bash
pi install /path/to/qstack        # local path
# or, once published:
pi install git:github.com/<you>/qstack
```

`pi list` confirms the package is installed. Project-local alternative: the repo already carries the skills under `.agents/skills/`, so running pi inside a project that vendors them needs no install at all — just approve the project-local files when prompted (or start once with `pi --approve`).

## Pick your models

Run:

```text
/skill:setup-pstack
```

[`/skill:setup-pstack`](../../.agents/skills/setup-pstack/SKILL.md) runs `pi --list-models` to detect the models you have access to, asks for a reasoning budget, shows you each role (code delegates, judgment, the review panels), and asks what you want. Answer the questions. It writes the `pstack models` section of `~/.pi/agent/AGENTS.md`, a small always-loaded config every pstack skill reads.

You only override what you care about. A role with no line in the section keeps the skill's default: the seat runs on your own model. To restore that default, delete the role's line. A rerun of `/skill:setup-pstack` keeps any role whose model you changed.

Set a role to `inherit-parent` or `auto` and pstack spawns the seat on your own `--provider`/`--model`, so it runs on the same model as this session. Both values mean the same thing, and neither is a model entry. For a panel role the value is a list, and one subagent runs per entry, so the list length sets the panel size. Setup also configures `swarm workers`, the default model for every `/skill:swarm` worker unless a race names a model for each arm.

## Accept the verification offer, or don't

At the end of setup, `/skill:setup-pstack` looks for a way to prove app behavior in your project, either a `verify-*` skill or an existing harness. If it finds neither, it offers once to generate one with [`/skill:create-verification-skill`](../../.agents/skills/create-verification-skill/SKILL.md).

Say yes and it writes `.agents/skills/verify-<app>/`, a project-local skill that teaches agents to drive your app the way a user does. It proves the skill works once before handing it over. Say no and setup moves on. You can run `/skill:create-verification-skill` yourself any time. [Verify and ship](./06-verify-and-ship.md#create-a-project-verification-skill) covers when it earns its place.

After setup, new spawns pick the section up immediately. Restarts are not required.

## Run your first task

Pick something real but small, and describe it the way you'd describe it to a colleague:

```text
/skill:poteto-mode add a --json flag to this command. text output stays byte-identical. verify both.
```

Watch the todo list. Its first items are the matched playbook's steps copied in, the Feature playbook for this prompt. If `/skill:poteto-mode` skips a step, the step stays in the list with `skip: <reason>`, so you can see what it chose not to do.

From here you can type normal follow-ups. `/skill:poteto-mode` is sticky. It stays on for the conversation until you opt out by saying so.

Next: [Route work through `/skill:poteto-mode`](./02-poteto-mode.md).
