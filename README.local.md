# Instatic — local setup (fork)

Fork of [CoreBunch/Instatic](https://github.com/CoreBunch/Instatic). Self-hosted
visual CMS. This file documents how *this* clone is run.

## Remotes

- `origin`   → `git@github.com:selnull/Instatic.git` (your fork — push here)
- `upstream` → `git@github.com:CoreBunch/Instatic.git` (pull updates from here)

```sh
git fetch upstream && git merge upstream/main   # pull upstream changes
```

## Run in Docker (SQLite, built from local source)

Requires Docker Desktop with **WSL integration enabled** for this distro
(Docker Desktop → Settings → Resources → WSL Integration).

```sh
docker compose -f compose.prod.yml -f compose.sqlite.yml -f compose.build.yml up -d --build
```

- Admin: http://localhost:3001/admin
- DB: `data` volume (`sqlite:/app/data/cms.db`)
- Uploads: `uploads` volume
- `.env` holds `INSTATIC_SECRET_KEY` (already generated, gitignored)

Rebuild after editing source: rerun the same command (`--build` picks up changes).

Stop / logs / reset:

```sh
docker compose -f compose.prod.yml -f compose.sqlite.yml -f compose.build.yml down
docker compose -f compose.prod.yml -f compose.sqlite.yml -f compose.build.yml logs -f app
docker compose -f compose.prod.yml -f compose.sqlite.yml -f compose.build.yml down -v   # also wipes DB + uploads
```

## Fast iteration on code (no Docker)

Vite HMR, SQLite at `.tmp/dev.db`, zero config:

```sh
bun install
bun run dev        # http://localhost:5173
```

Use this while changing source; use Docker for the prod-like run.

## Building websites

Each site is created inside the visual editor at `/admin` — no code per site.
Editing this repo's code only changes the CMS/editor itself.
