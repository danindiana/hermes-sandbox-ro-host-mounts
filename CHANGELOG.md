# Changelog

## 2026-10-07
- Added three read-only host mounts (`/ro/Documents`, `/ro/Downloads`, `/ro/research_notes`) to the Hermes Docker sandbox.
- Masked one `.env` holding a real API key with a `/dev/null` bind mount.
- Removed the old sandbox container; CLI recreated it with the new mounts (verified from the host).
- Restarted `hermes-gateway` (user unit) so it reads the new config; removed the stale container it had created.
- Verified the gateway path with a one-shot cron job (no messaging platform is enabled, so a chat test was impossible).
- Published this write-up.

- Investigated the cron import errors (stale-process after hermes update) and the intermittent cronjob_tools deadlock; see README open items.

- Added a /ro path-translation section to the workspace AGENTS.md after the agent looked for /home/smduck/Downloads (host path) and reported it missing.

- Live-tested the AGENTS.md path fix: agent used /ro/Downloads correctly but misread ls -ltr ordering.

- Added an ls -lt / -ltr sorting hint to the workspace AGENTS.md (in context, but the model still ignored it in a live retest).

- Added helpers/newest (installed at /workspace/bin/newest) and pointed AGENTS.md at it; live retest returned the correct newest files.
