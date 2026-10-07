# Config diff

`~/.hermes/config.yaml`, `terminal.docker_volumes` (secret-bearing path generalized):

```diff
   docker_volumes:
     - ~/Documents/hermes-sessions:/workspace:rw
     - ~/.hermes/sandbox_ssh:/home/pn/.ssh:rw
+    - ~/Documents:/ro/Documents:ro
+    - ~/Downloads:/ro/Downloads:ro
+    - ~/research_notes:/ro/research_notes:ro
+    - /dev/null:/ro/Documents/<path-to>/remote_apply_agent/.env:ro
```

Nothing else changed. The raw pre-change config is kept locally and deliberately not published.
