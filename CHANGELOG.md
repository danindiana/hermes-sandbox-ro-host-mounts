# Changelog

## 2026-10-07
- Added three read-only host mounts (`/ro/Documents`, `/ro/Downloads`, `/ro/research_notes`) to the Hermes Docker sandbox.
- Masked one `.env` holding a real API key with a `/dev/null` bind mount.
- Removed the old sandbox container; the CLI recreated it with the new mounts (verified from the host).
- Restarted `hermes-gateway` (user unit) so it reads the new config; removed the stale container it had created.
- Verified the gateway path with a one-shot cron job (no messaging platform is enabled, so a chat test was impossible).
- Published this write-up with 8 diagrams and a logo.
- Investigated the cron errors: `is_recurring` ImportError was a stale gateway after `hermes update`; `cronjob_tools` deadlock was an intermittent startup race.
- Added a path-translation section to the workspace `AGENTS.md` after the agent searched `/home/<user>/Downloads` and reported it missing.
- Added a sorting hint (ignored by the model), then `helpers/newest` installed at `/workspace/bin/newest`; live retest returned the correct newest files.
- Restarted `hermes-gateway` again (13:10:11); the deadlock did not recur. Tested the `cronjob` tool: registry check passes; live-agent paths were fabricated (CLI) or blocked by design (cron session).
- Final pass: diagrams 09-10, README restructure, runbook rewrite, session record. User confirmed everything works on their running Hermes instance.
