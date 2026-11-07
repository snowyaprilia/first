# jango-bulk via Dockge

Minimal Docker packaging for https://codeberg.org/0xgetz/jango-bulk, so it can
run as a stack in your existing Dockge instance. No new sandbox needed — this is
just a `compose.yaml` you paste into Dockge.

## Why a Dockerfile is added

Upstream ships no Dockerfile, only a `Makefile`. Dockge runs Compose stacks, so
we build the Go binary inside a container and run it with `serve`.

## How to use in Dockge

1. In Dockge, click **+ Compose** (top right) and name the stack `jango-bulk`.
2. Paste the contents of `docker/compose.yaml` into the stack editor (the file
   sits at the stack root, next to `repo/` and `docker/`).
3. Click **Deploy**. Dockge runs `docker compose up -d --build` for you.

## Files

- `docker/Dockerfile` — multi-stage build (golang:1.23-alpine → alpine:3.20), runs as non-root user `app` (uid 1000).
- `docker/compose.yaml` — the stack definition.
- `docker/.env.example` — optional env vars; rename to `.env` if you need overrides.

## Notes

- Default bridge command is `serve --addr 0.0.0.0:20581` so it is reachable from
  the host and other containers (e.g. 9Router) via `host.docker.internal` or the
  Docker network. Upstream default `127.0.0.1:20581` would be unreachable from
  outside the container.
- Account store lives in `./data` (mounted to `/data/data` inside the container),
  so sessions persist across restarts.
- For the full pipeline (signup accounts, register into 9Router) use Dockge's
  terminal or `docker compose exec jango-bulk jango-bulk run -n 5 --router http://<9router>:20128`.
