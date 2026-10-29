# Tests

A skill is a prompt, so it is tested by running it, not by asserting on functions. Three checks, in
increasing cost:

## 1. Structure (free, run on every change)

```bash
# the example must parse and stay inside the budget
python - <<'PY'
import re, pathlib
p = pathlib.Path("examples/2026-10-29-tv-debloat.md")
t = p.read_text(encoding="utf-8")
fm = re.match(r"^---\n(.*?)\n---\n", t, re.S)
assert fm, "missing front matter"
keys = {l.split(":", 1)[0] for l in fm.group(1).splitlines() if ":" in l}
assert keys == {"date", "task", "status", "supersedes", "verified"}, keys
for h in ("Where it stopped", "Next step", "Decisions", "Dead ends", "Artifacts", "Boundaries",
          "Open questions"):
    assert f"## {h}" in t, h
assert len(t.splitlines()) <= 60, len(t.splitlines())
assert not re.search(r"sk-[A-Za-z0-9]{8,}|ghp_[A-Za-z0-9]{8,}|Bearer\s+[A-Za-z0-9._-]{10,}", t), \
    "secret-shaped string"
print("ok")
PY
```

## 2. Write a handoff (manual, 2 minutes)

Start a session, do ~10 minutes of work with a decision and one failed attempt, then say
`交接一下`. Pass if: a file appears with front matter, all seven headings, the dead end recorded, every
artifact marked verified or unverified, and the whole thing under 60 lines.

## 3. Resume (the one that matters)

Open a **fresh** session with only the handoff file (no chat history), say `接上次`. Pass if the agent:
restates goal / stop point / next step, runs or explicitly requests the staleness check, does not
re-ask an answered question, does not retry a dead end, and stops when the next step's definition of
done holds. Fail cases seen in the wild: it summarises instead of resuming; it ignores `unverified`; it
invents progress to look productive.

## Known failure modes

- Harnesses that hide the transcript from the model: the handoff inherits the gap. It is why every
  artifact carries a verified flag.
- Very terse users: "ok" is not a handoff request. The skill asks once, then drops it.
- Handoffs in synced folders: they can leak business context to a cloud drive. The default target is a
  local `.handoff/` directory for this reason.
