# session_1791394650: Hermes sandbox read-only host mounts

Folder name = `date +%s` at session start (2026-10-07). Repo: https://github.com/danindiana/hermes-sandbox-ro-host-mounts
(this folder is the repo root).

## Goal
Let the Hermes Agent Docker sandbox read the user's saved work (Documents, Downloads) without being able to change it.

## Timeline
1. Discussed risk: read-only stops writes, not exfiltration (sandbox has internet egress). Chose an allowlist, not `$HOME`.
2. Secret scan (names, symlinks, content patterns). One real `.env` found and masked with `/dev/null`.
3. Edited `terminal.docker_volumes`; recreated the container; verified from the host (`docker inspect`/`exec`).
4. Gateway kept a stale container from the old config: restarted `hermes-gateway` (`systemctl --user`), removed the stale one,
   verified through a one-shot cron job (no messaging platform exists to chat with).
5. Published the repo (README, runbook, diagrams, logo).
6. Investigated the cron errors (stale process after `hermes update`; intermittent startup race).
7. Agent looked for the host path: added an `AGENTS.md` path table; sort-order mistakes: added `helpers/newest`.
8. Restarted the gateway again; tested the `cronjob` tool as far as possible. Final docs pass.

## Changed outside this repo
* `~/.hermes/config.yaml`: four volume lines added (raw pre-change copy kept locally as `config.yaml.bak`, gitignored).
* `~/Documents/hermes-sessions/AGENTS.md`: `/ro` path table and helper instructions.
* `~/Documents/hermes-sessions/bin/newest`: helper script (copy in `helpers/`).
* `~/Documents/hermes-sessions/HERMES_DOCKER_SANDBOX_REFERENCE.md`: `/ro/*` and helper notes.
* Containers recreated; `hermes-gateway` restarted twice. Two temporary cron jobs created and self-removed.

## Outcome
Working, confirmed by the user on their running Hermes instance.

## Open items
* Rotate the API key that sat in the masked `.env` (user action).
* `cron.allow_agent_scheduling` left off on purpose.
* An end-to-end `cronjob` tool call from a real gateway chat session is untested (needs a messaging platform).
