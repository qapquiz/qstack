---
name: poteto-herdr-mode
description: "poteto's agent style with every subagent running in a visible Herdr tab the operator can watch live, while the parent still reads the same output-file reports as poteto-mode. Use for /skill:poteto-herdr-mode or when spawning poteto subagents inside Herdr (HERDR_ENV=1)."
disable-model-invocation: true
---

# Poteto herdr mode

Poteto herdr is poteto-mode with every subagent running in a Herdr tab. The operator watches live. The parent reads the same report as always. Sticky like poteto-mode. Once entered it stays on across turns. A new task, a playbook match, or rigor needed applies it. A casual turn, or the operator opting out, does not.

## Base

Read `../poteto-mode/SKILL.md` in full. It supplies the principles index, the non-negotiable triggers, autonomy rules, reply and comment rules, and the playbook index. All of it applies here, with one replacement below.

## Replacement: the spawn runs in a tab

poteto-mode's Subagents section stands, except its "background it" default. The poteto-agent briefing, per-role models, tooling contract, file pointers, and fresh-spawn rules are unchanged. Only where the process lives changes.

1. **Gate.** Run `test "${HERDR_ENV:-}" = 1`. On failure, use poteto-mode's plain background spawn. No herdr command works from outside Herdr.

2. **Workspace, once per session.** Spawn tabs live in one dedicated workspace so they never litter the operator's working tabs. Create it if missing and reuse it for every spawn.

```bash
herdr workspace create --label poteto-spawns --no-focus
```

3. **Spawn.** One tab per subagent, labeled with the role, never focused.

```bash
herdr tab create --workspace <ws> --label <role> --no-focus
herdr pane run <pane> 'pi -p --no-session --append-system-prompt <briefing> \
  [--provider <p>] [--model <m>[:<thinking>]] [--tools read] \
  "<task>" > /tmp/<name>.out 2>&1; echo "TAB""_DONE"'
```

4. **Wait.** Start every spawn before waiting on any, then block on a line-anchored marker per spawn.

```bash
herdr pane wait-output <pane> --regex '^TAB_DONE$' --timeout 3600000
```

The quote-split `"TAB""_DONE"` is mandatory. The shell echoes the command line before running it, so a plain marker matches the echo, the wait returns early, and the parent reads an empty file.

5. **Read, then close.** The parent reads `/tmp/<name>.out`, the same artifact a background spawn produces, and reviews it per poteto-mode. Then close the tab.

```bash
herdr tab close <tab_id>
```

Never close before the marker arrives. Closing the tab kills the run.

## Watching

The operator clicks into any spawn tab to watch the subagent work. The parent never reads a tab for content. `herdr pane read <pane>` is for the operator's eyes and for debugging a spawn whose wait timed out. A timeout means stuck, not dead. Read the pane before deciding to close.
