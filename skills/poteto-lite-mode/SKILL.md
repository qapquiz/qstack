---
name: poteto-lite-mode
description: "poteto's agent style for concise, detailed responses, unslopped prose, simple code, and verified work, all run by a single agent with no subagents and no multi-model seats. Use for /skill:poteto-lite-mode or when the operator wants the style without delegation."
disable-model-invocation: true
---

# Poteto lite mode

Poteto lite is poteto-mode run by one agent. You do every step yourself. Sticky like poteto-mode. Once entered it stays on across turns. A new task, a playbook match, or rigor needed applies it. A casual turn, or the operator opting out, does not.

## Base

Read `../poteto-mode/SKILL.md` in full. It supplies the principles index, the non-negotiable triggers, autonomy rules, reply and comment rules, and the playbook index. All of it applies here, with the replacements below.

## Replacements

- **Subagents.** poteto-mode's Subagents section is out. No pi spawns, no `poteto-agent` briefings, no per-role model tiers. Every step runs in your own thread.
- **`how`, `why`, `reflect`.** Run the exploration or analysis yourself in one pass. No explorer spawns, no diverse-model review.
- **`architect`.** Sketch types, signatures, and module boundaries yourself. Explore 2 to 3 designs inline, pick one, and record why (the **exhaust-the-design-space** principle skill applies, just sequentially).
- **`interrogate`.** Replaced by a written red-team pass on your own design. Name the strongest objection, then answer it or change the design.
- **`swarm` and `arena`.** Out of scope. Fan-out work runs as a checklist you execute item by item.
- **Delegation steps in playbooks.** Where a playbook step says delegate to a subagent, do that work yourself and hold it to the playbook's own review bar.
- **Guard the Context Window.** No delegate takes bulk off your hands. Write bulk to a file and read back slices. Keep summaries in the thread.
- **Dropped playbooks.** `orchestrate` and `autopilot-full` coordinate fleets of agents, so lite never matches them. Route that shape of work to poteto-mode.

## Playbooks

Match a playbook from poteto-mode's index and open its steps from `../poteto-mode/playbooks/<title>.md`, with the title in kebab case, hyphens kept and other punctuation dropped. Execute every step yourself. A step you choose not to do stays in the list with a one-line `skip: <reason>`.
