# Changelog

## 2026-10-07
- Added three read-only host mounts (`/ro/Documents`, `/ro/Downloads`, `/ro/research_notes`) to the Hermes Docker sandbox.
- Masked one `.env` holding a real API key with a `/dev/null` bind mount.
- Removed the old sandbox container; CLI recreated it with the new mounts (verified from the host).
- Restarted `hermes-gateway` (user unit) so it reads the new config; removed the stale container it had created.
- Verified the gateway path with a one-shot cron job (no messaging platform is enabled, so a chat test was impossible).
- Published this write-up.
