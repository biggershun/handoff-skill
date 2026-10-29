---
date: 2026-10-29T15:58+08:00
task: strip ads and telemetry from a Sony 75X9500H (Android TV, CN firmware)
status: active
supersedes: none
verified: 2026-10-29
---

## Where it stopped
The debloat toolkit is deployed and its Python environment verified: the probe, check and adb-closed
tools all print usage (`--help`, exit 0). Evidence: three `--help` runs, 15:22 local.
The TV has **not** been touched — no device has been connected yet.

## Next step
Read the TV's platform over ADB, then run the read-only package check against the candidate list.
Done when: the reported platform matches the list's platform (BRAVIA_VH1 family), and every package in
the list is classified as present / already-disabled / absent.

## Decisions
- Read-only check first, disable later — because a wrong list can remove remote support and OTA.
- Keep the stock launcher enabled as a fallback — because the replacement launcher is optional.
- Use the same-generation device list rather than a generic Sony one — because the platform differs.

## Dead ends
- Enabling HTTPS on the router — the switch is tailnet-level, in the account settings, not on the box.
- Judging "is it configured" from the human-readable serve status — that output never contains the port,
  so a watchdog re-configured every 10 minutes and spammed the user.

## Artifacts
- `D:\Tools\cleanyourtv` — verified (venv + deps + CLI usage)
- `D:\Tools\cleanyourtv\examples\sony-x9000h-cn\debloat.txt` — unverified against the real TV
- `D:\Tools\cleanyourtv\README-本地部署.md` — verified (written, 2.2 KB)

## Boundaries
- Already authorized: enabling debugging on the TV, read-only probing.
- Must re-confirm: disabling any system package; anything that needs the TV's admin UI.

## Open questions
- Replace the stock launcher with an Apple-TV-style one, or only debloat? (decides whether a launcher
  build/install happens at all)
