# Runbook

## Apply
1. Back up `~/.hermes/config.yaml`.
2. Run the secret scan (README, "Secret scan"). Mask or exclude anything it finds.
3. Add the `:ro` lines under `terminal.docker_volumes` (see `config-diff.md`).
4. Remove the running sandbox container: `docker rm -f <hermes-container>`. It is persistent, so edits do nothing until then.
5. Restart anything that cached the config: `systemctl --user restart hermes-gateway` (a **user** unit; `sudo systemctl` says "not found").
6. Remove any container created from the stale config.

## Teach the agent the paths
1. Add a path table to the workspace `AGENTS.md` (Documents, Downloads, research_notes to `/ro/...`; `/ro` is read-only; copy to
   `/workspace` to modify; translate `/home/<user>/...` paths).
2. Install `helpers/newest` as `<workspace>/bin/newest` (`chmod +x`) and point `AGENTS.md` at it for newest/oldest questions.
3. New sessions pick this up; no restart needed.

## Verify (from the host, not from the model)
```sh
docker inspect <container> --format '{{range .Mounts}}{{.Source}} -> {{.Destination}} rw={{.RW}}{{"\n"}}{{end}}'
docker exec <container> sh -c 'touch /ro/Documents/x; touch /workspace/.t && echo ws_rw_ok && rm /workspace/.t; ls -d /ro/.ssh /ro/.config'
docker exec <container> /workspace/bin/newest 3 /ro/Downloads
```
Expect: `/ro/*` show `rw=false`; the first `touch` says "Read-only file system"; `ws_rw_ok`; the `ls` finds nothing; `newest`
prints the newest files first.
Gateway: `grep 'Docker volume_args' ~/.hermes/logs/agent.log | tail -1` shows the `/ro/...:ro` entries.

Testing through a Hermes chat: use `hermes chat -t terminal -q ...` and confirm the session reports tool calls > 0
(`sqlite3 ~/.hermes/state.db "select tool_calls from messages where session_id='<id>'"`). Without tool calls the local model
may invent output.

## After `hermes update`
Restart the long-lived processes: `systemctl --user restart hermes-gateway`. A gateway started before the update caches old
modules and fails with `cannot import name ...` when it loads newer files. If a startup shows
`Could not import tool module tools.cronjob_tools: deadlock detected`, restart again (intermittent race).

## Caveats
* Cron-spawned agents never get the `cronjob` tool (loop prevention) unless `cron.allow_agent_scheduling: true`.
* `hermes cron create --workdir` is validated on the host, so container paths like `/workspace` are rejected.
* The container clock is UTC; host times are CDT.
* The `/dev/null` mask points at one file. If it is moved or deleted, container start fails: remove the mask line.

## Rollback
Delete the added lines, `docker rm -f` the container, restart the gateway.
