# Changelog

## 2.0.0 — 2026-10-29

Rewritten for publication: harness-neutral, path-configurable, machine-readable.

- One Markdown file, no scripts or dependencies; works in chat UIs as well as coding agents.
- Natural-language triggers in English and Chinese.
- Five-key front matter (`date`, `task`, `status`, `supersedes`, `verified`) so handoffs can be found,
  ordered and superseded by a script or an agent.
- Required `Dead ends` section; required `Boundaries` section (authorized vs must-re-confirm).
- `RESUME` gained a staleness check and a finish condition.
- `VERIFY` mode added (completeness, staleness, riskiest unverified claim).
- Secrets: concrete key shapes, redaction, and a fail-closed rule (refuse to write rather than leak).
- Paths: defaults to `.handoff/` in the working directory, asks once if the user keeps notes elsewhere;
  never writes to `/tmp` or a synced folder by default.

## 1.0.0

Private, single-machine version: fixed paths, Chinese only, no front matter, no verify mode.
