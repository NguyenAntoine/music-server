# Navidrome Setup

Navidrome is a lightweight, self-hosted music streaming server that supports the Subsonic API and can stream music from a folder on your server.

## Docker Compose Configuration

```yaml
services:
  navidrome:
    image: deluan/navidrome:latest
    container_name: music_navidrome
    restart: always
    expose:
      - 4533
    volumes:
      - ./data:/data
      - ${MUSIC_FOLDER}:/music:ro
    environment:
      - ND_MUSICFOLDER=/music
      - ND_DATAFOLDER=/data
      - ND_LOGLEVEL=info
      - ND_SESSIONTIMEOUT=24h
      - VIRTUAL_HOST=${VIRTUAL_HOST}
      - LETSENCRYPT_HOST=${LETSENCRYPT_HOST}
      - LETSENCRYPT_EMAIL=${LETSENCRYPT_EMAIL}
      - VIRTUAL_PORT=4533
    networks:
      - default
      - reverse-proxy

networks:
  reverse-proxy:
    external: true
```

## Setup Instructions

1. Copy the environment template:
   ```bash
   cp .env.dist .env
   ```

2. Edit `.env` and fill in:
   - `VIRTUAL_HOST`: Your domain (e.g., music.example.com)
   - `LETSENCRYPT_HOST`: Same as VIRTUAL_HOST
   - `LETSENCRYPT_EMAIL`: Your email for Let's Encrypt
   - `MUSIC_FOLDER`: Absolute path to your music folder on VPS (e.g., /mnt/storage/music)

3. Transfer to VPS:
   ```bash
   rsync -av /Users/antoine/Perso/music-server/ vps:/path/to/music-server/
   ```

4. On VPS, start the container:
   ```bash
   cd /path/to/music-server
   docker compose up -d
   ```

5. Access the web UI at `https://VIRTUAL_HOST` and create your admin account

## Features
- Lightweight music streaming server
- Subsonic API compatible (works with many Subsonic clients)
- Web UI for music browsing and playback
- SQLite-based (no external DB needed)
- Read-only mount of music folder
