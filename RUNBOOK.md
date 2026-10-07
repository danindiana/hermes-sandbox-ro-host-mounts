# Runbook

## Apply
1. Back up `~/.hermes/config.yaml`.
2. Run the secret scan (README, "Secret scan"). Mask or exclude anything it finds.
3. Add the `:ro` lines under `terminal.docker_volumes` (see `config-diff.md`).
4. Remove the running sandbox container: `docker rm -f <hermes-container>`. It is persistent, so edits do nothing until then.
5. Restart anything that cached the config: `systemctl --user restart hermes-gateway` (a **user** unit; `sudo systemctl` says "not found").
6. Remove any container created from the stale config.

## Verify (from the host, not from the model)
```sh
docker inspect <container> --format '{{range .Mounts}}{{.Source}} -> {{.Destination}} rw={{.RW}}{{"\n"}}{{end}}'
docker exec <container> sh -c 'touch /ro/Documents/x; touch /workspace/.t && echo ws_rw_ok && rm /workspace/.t; ls -d /ro/.ssh /ro/.config'
```
Expect: `/ro/*` show `rw=false`; the first `touch` says "Read-only file system"; `ws_rw_ok`; the `ls` finds nothing.
Gateway: `grep 'Docker volume_args' ~/.hermes/logs/agent.log | tail -1` shows the `/ro/...:ro` entries.
If testing through a Hermes chat, use `hermes chat -t terminal -q ...` and check the session reports tool calls > 0.

## Rollback
Delete the four added lines, `docker rm -f` the container, restart the gateway.

## Known fragility
The `/dev/null` mask points at a specific file. If that file is moved or deleted, container start fails; remove the mask line.
