# Filebrowser — Docker Compose Setup

This repository provides a standardized Docker Compose configuration for File Browser, allowing quick deployment with consistent volume paths and environment variable handling.

## What This Provides

A clean, repeatable structure for deploying File Browser with persistent configuration, predictable permissions, and reproducible environment variables across your self‑hosted environments.

## Features

- Consistent directory layout
- Persistent database and settings files
- Customizable user and group IDs
- Simple one‑command startup
- Ready for reverse‑proxy integration

## Getting Started

Clone the repository:

```
git clone https://github.com/paulkakell/filebrowser-dockercompose
cd filebrowser-dockercompose
```

Create required persistence files:

```
touch filebrowser.db
touch settings.json
```

Start the container:

```
docker compose up -d
```

Visit File Browser:

```
http://<host-ip>:8080
```

Default login: admin / admin (change immediately)

## Docker Compose Example

```
version: "3"

services:
  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: filebrowser
    user: "${PUID:-1000}:${PGID:-1000}"
    volumes:
      - ./data:/srv
      - ./filebrowser.db:/database/filebrowser.db
      - ./settings.json:/config/settings.json
    ports:
      - "8080:80"
    environment:
      - PUID=${PUID:-1000}
      - PGID=${PGID:-1000}
    restart: unless-stopped
```

## Notes

- `PUID` and `PGID` should match the user/group that owns your data directories.
- The `data` folder becomes the root visible inside the File Browser UI.
- `filebrowser.db` and `settings.json` must be mounted; otherwise all settings reset on container recreation.
- Works well behind reverse proxies like NPM, Traefik, or Caddy.

## License

This repository is licensed under the Unlicense.
