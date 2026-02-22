# Navidrome

A lightweight, self-hosted music streaming server that supports the Subsonic API. Stream your personal music collection from anywhere.

## Features

- 🎵 Stream your music collection
- 📱 Subsonic API compatible (works with many mobile apps)
- 🌐 Beautiful web UI for browsing and playback
- 🎙️ Lightweight and fast
- 🔐 Self-hosted - complete control over your music
- 🗄️ SQLite database (no external DB needed)

## Quick Start

1. Copy the environment template:
   ```bash
   cp .env.dist .env
   ```

2. Edit `.env` and fill in your configuration:
   - `VIRTUAL_HOST`: Your domain (e.g., music.example.com)
   - `LETSENCRYPT_HOST`: Same as VIRTUAL_HOST
   - `LETSENCRYPT_EMAIL`: Your email for Let's Encrypt certificates
   - `MUSIC_FOLDER`: Absolute path to your music folder on the server (e.g., `/mnt/storage/music`)

3. Start the service:
   ```bash
   docker compose up -d
   ```

4. Access the web UI at `https://VIRTUAL_HOST` and create your admin account

## Music Folder Setup

Ensure your music folder is accessible to the container:

```bash
# Example: ensure proper permissions
chmod -R 755 /mnt/storage/music
chown -R 1000:1000 /mnt/storage/music  # Adjust user/group as needed
```

Supported formats: MP3, FLAC, OGG, M4A, WAV, WMA, and more.

## Updating

To update to the latest image:

```bash
./updateDockerImages.sh
```

## Architecture

- **Container**: `music_navidrome` (port 4533)
- **Volumes**:
  - `./data:/data` (SQLite database and cache)
  - `${MUSIC_FOLDER}:/music:ro` (Read-only music folder)
- **Network**: Connected to `reverse-proxy` network for HTTPS via nginx-proxy

## Subsonic Clients

Once running, you can use Navidrome with Subsonic-compatible clients:
- **Mobile**: Subsonic, Subtracks, Tempo, etc.
- **Desktop**: Sublime Music, Gnome Musik, etc.
- **Web**: Built-in Navidrome web UI

Configure client to connect to: `https://VIRTUAL_HOST`

## Configuration

See `.claude/plans/init.md` for full setup instructions and Docker Compose details.

## Links

- [Navidrome GitHub](https://github.com/navidrome/navidrome)
- [Navidrome Documentation](https://www.navidrome.org/)
- [Subsonic API Documentation](http://www.subsonic.org/pages/api.jsp)
