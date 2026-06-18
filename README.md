# Spectre Music Search Bot 🎵

A production-ready, containerized Discord bot built with Python (`discord.py`) and PostgreSQL. It monitors specified text channels for music links, parses streaming and YouTube metadata, and maintains a persistent database ledger of your community's track mashups and shared music.

---

## 🚀 Quick Start (Docker Deployment)

This project is fully containerized using Docker Compose, allowing you to deploy the bot and its database with a single command.

### 1. Prerequisites
Make sure you have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your host machine.

### 2. Configuration
Clone this repository to your server, then open the `docker-compose.yml` file. You will need to replace the placeholder values in the environment blocks with your own configuration:

```yaml
    environment:
      - DISCORD_TOKEN=your_bot_token_here          # From Discord Developer Portal
      - OWNER_DISCORD_IDS=123456789,987654321      # Comma-separated admin user IDs
      - MASHUP_CHANNEL_IDS=1122334455,6677889900   # Comma-separated channel IDs to monitor
      - DB_HOST=mashup_postgres                    # Keep as service name
      - DB_NAME=mashup_db                          # Must match POSTGRES_DB below
      - DB_USER=postgres                           # Must match POSTGRES_USER below
      - DB_PASSWORD=your_secure_password           # Must match POSTGRES_PASSWORD below
