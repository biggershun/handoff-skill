# Handoff

**One Markdown file that lets any model, in any session, pick up exactly where the last one stopped —
without redoing work, re-asking questions, or walking into the dead ends you already ruled out.**

A handoff is not a summary. A summary tells you what happened. A handoff tells the next agent **what
is true now, what to do next, what not to repeat, and what is still unproven.**

```
You:   交接一下
Agent: wrote .handoff/2026-10-29-1558-tv-setup.md  (status: active, 38 lines)

--- new chat, another model, next morning ---

You:   接上次
Agent: Goal: strip ads from the living-room TV.
        Stopped: tool deployed to /path/to/toolkit, TV not touched yet.
        Next: read the TV's model over ADB, then the read-only package check.
        One line is stale: the ports-check output from yesterday no longer applies — re-running it.
```

## Why this one

Handoff prompts are easy to write badly. The common failures are predictable: the document becomes a
transcript, the dead ends get dropped (so the next agent retries them), "done" is asserted without
evidence, and secrets ride along. This skill is written against those failures, and it is deliberately
small enough that any model can follow it.

- **Universal.** One Markdown file. No scripts, no CLI, no dependencies, no vendor API. Works in a chat
  window, Claude Code, Codex, Cursor, Copilot, Gemini CLI, OpenCode, or your own agent.
- **Natural language.** Say `handoff` / `交接一下` / `接上次` / `pick up where I left off`. No special
  command syntax.
- **Facts are checked, not recalled.** Every artifact is either verified this turn or explicitly marked
  `unverified`.
- **Dead ends are a required section.** It is the section that saves the most time and the one every
  other format leaves out.
- **Machine-readable shell.** Five front-matter keys (`date`, `task`, `status`, `supersedes`,
  `verified`) so a script or an agent can find, order and supersede handoffs.
- **A resume contract.** The receiving agent restates the handoff, runs a staleness check, and knows
  when it is finished — instead of drifting.
- **Fail-closed on secrets.** Known key shapes are redacted; if something sensitive would still
  survive, the skill refuses to write the file.

## Compare

| | This | mattpocock `handoff` | Jiaojie 交接 | CatchUp | Superpowers |
|---|---|---|---|---|---|
| Format | 1 Markdown skill | 1 Markdown skill | skill + zip/audit modes | CLI + local session files | skill bundle |
| Dependencies | none | none | Python for packaging modes | Node CLI | plugin runtime |
| Cross-model / cross-harness | yes | yes | yes (many runtimes) | 14 harnesses, no Hermes | mostly Claude Code |
| Machine-readable front matter | yes | no | yes (coverage/status) | n/a (tool) | n/a |
| Staleness check on resume | required | no | revalidation noted | partial (re-reads session) | no |
| Reads the real session record | if the harness exposes it | no | no | **yes** | no |
| Dead ends required | yes | no | yes | no | no |
| Size | ~120 lines of prompt | ~16 lines | larger, multi-mode | tool | bundle |

Pick CatchUp if you want a CLI that forks a Claude Code session into Codex. Pick Jiaojie if you need
audit packaging and cross-organization delivery. Pick this if you want the smallest thing that both a
phone chat and a coding agent can follow, with a resume contract that does not drift.

## Install

Copy `SKILL.md` wherever your agent reads skills. Nothing else is required.

| Agent | Where it goes |
|---|---|
| **Hermes Agent** | `skills/handoff/SKILL.md`, or point your profile's skills dir at it |
| **Claude Code** | `~/.claude/skills/handoff/SKILL.md` (or a project `.claude/skills/`) |
| **Codex / OpenCode** | project `AGENTS.md` — append this file's body, or keep it as `skills/handoff/SKILL.md` if your setup loads skills |
| **Cursor / Copilot / Windsurf** | paste the body into `.cursor/rules/handoff.mdc` or `.github/copilot-instructions.md` |
| **Gemini CLI / any chat UI** | upload `SKILL.md`, or paste it as the first message of the session |

`AGENTS.md` in this repo is the same content in the "just paste me into a rules file" shape.

## The file it writes

```markdown
---
date: 2026-10-29T15:58+08:00
task: strip ads from the living-room TV
status: active
supersedes: none
verified: 2026-10-29
---

## Where it stopped
Tooling deployed to /path/to/toolkit; venv + deps verified (`--help` on all three tools).
The TV itself has not been touched. Evidence: `python tools/tv.py --probe` has not run yet.

## Next step
Read the TV's model over ADB, then run the read-only package check.
Done when: the reported platform matches the candidate debloat list.

## Decisions
- Read-only check before any disable — because a wrong list removes remote support.

## Dead ends
- Enabling HTTPS on the router — the switch is tailnet-level, not on the router.

## Artifacts
- /path/to/toolkit — verified
- examples/sony-x9000h-cn/debloat.txt — unverified (not yet compared to the TV)

## Boundaries
- Already authorized: read-only probing.
- Must re-confirm: disabling system packages.

## Open questions
- Replace the stock launcher, or only debloat? (changes whether a launcher gets installed)
```

Sixty lines is the ceiling and most real handoffs land at 30–45.

## What it will not do

- It will not read a session it cannot see. Harnesses that hide their transcript give it only the
  visible conversation — mark those claims `unverified`.
- It will not move files between machines. References yes, payload no. Use a zip or a sync tool.
- It will not replace context compression. Compression keeps a long session alive; this carries work
  across sessions.

## Credits

Ideas taken from `mattpocock/skills` (the minimal handoff), `Jordanwei1/jiaojie-skill` (dead ends,
revalidation, format choice), `wilbeibi/catchup` (read the real session record) and `obra/superpowers`
(skills that state their own failure modes). See [ATTRIBUTION.md](ATTRIBUTION.md).

## Author

**biggershun (大顺)** — [github.com/biggershun](https://github.com/biggershun)

Search terms: handoff skill, agent handoff, session handoff, context handoff, AI 交接, 交接技能,
上下文交接, 换会话继续, resume agent session, Claude Code skill, Codex skill, Hermes Agent skill,
portable agent skill, prompt-only skill.

MIT.