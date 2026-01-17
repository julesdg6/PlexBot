# PlexBot Docker Configuration

This directory contains the Docker configuration files for running PlexBot with Lavalink.

## Files in This Directory

- **`dockerfile`** - Defines the PlexBot container image
- **`docker-compose.yml`** - Orchestrates PlexBot and Lavalink services
- **`lavalink.application.yml`** - Lavalink audio server configuration
- **`plugins/`** - Directory for Lavalink plugins (e.g., YouTube source, SoundCloud, etc.)

## Quick Start

From the project root directory:

```bash
# Configure environment variables
cp RenameMe.env.txt .env
nano .env  # Edit with your Discord token and Plex settings

# Start the services
cd Install/Docker
docker compose up -d
```

## Services

### PlexBot
- Connects to Discord and Plex
- Handles user commands and interactions
- Manages music playback through Lavalink

### Lavalink
- Audio streaming server
- Handles YouTube, SoundCloud, and other audio sources
- Communicates with PlexBot over the `plexbot-network`

## Configuration

### Lavalink Configuration (`lavalink.application.yml`)

The default configuration includes:
- **Port**: 2333
- **Password**: `youshallnotpass` (should match `LAVALINK_SERVER_PASSWORD` in `.env`)
- **Enabled Sources**: YouTube, Bandcamp, SoundCloud, Twitch, Vimeo, HTTP
- **Search**: YouTube and SoundCloud search enabled

You can customize this file to:
- Change the password
- Enable/disable audio sources
- Adjust buffer settings
- Configure logging levels

### Environment Variables

Key environment variables in your `.env` file:
- `DISCORD_TOKEN` - Your Discord bot token
- `PLEX_URL` - Your Plex server URL
- `PLEX_TOKEN` - Your Plex authentication token
- `LAVALINK_HOST` - Hostname for Lavalink (default: `lavalink`)
- `LAVALINK_SERVER_PASSWORD` - Password for Lavalink (must match `lavalink.application.yml`)
- `LAVALINK_SERVER_PORT` - Lavalink port (default: 2333)

### Lavalink Plugins

Place Lavalink plugin `.jar` files in the `plugins/` directory. They will be automatically loaded by Lavalink on startup.

Popular plugins:
- LavaSrc (Spotify, Apple Music, Deezer support)
- LavalinkServer (additional audio sources)

## Managing the Containers

```bash
# View logs
docker compose logs -f

# Stop services
docker compose down

# Restart services
docker compose restart

# Rebuild after code changes
docker compose up -d --build
```

## Troubleshooting

### Lavalink Won't Connect

1. Check that Lavalink is running: `docker compose ps`
2. Verify the password in `.env` matches `lavalink.application.yml`
3. Check logs: `docker compose logs lavalink`

### PlexBot Can't Find Lavalink

1. Ensure both containers are on the same network: `docker network inspect plexbot-network`
2. Verify `LAVALINK_HOST` is set to `lavalink` (the service name)
3. Check that Lavalink started before PlexBot: `docker compose ps`

## For More Information

See the [Docker Guide](../../Docs/Setup/Docker-Guide.md) for detailed documentation.
