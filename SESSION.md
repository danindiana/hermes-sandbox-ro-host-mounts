# session_1791394650: Hermes sandbox read-only host mounts

Folder name = `date +%s` at session start. Repo: danindiana/hermes-sandbox-ro-host-mounts.

- Added `:ro` mounts of Documents, Downloads, research_notes at `/ro/*`; masked one secret `.env`.
- Verified: CLI container (host-side `docker inspect`/`exec`) and gateway (agent.log volume args + cron run output).
- Corrections along the way are in README ("Corrections").
- Raw config backup stays local (`config.yaml.bak`, gitignored).
