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

```

> ⚠️ **Important:** The `DB_PASSWORD` under the `mashup_bot` service **must perfectly match** the `POSTGRES_PASSWORD` defined under the `mashup_postgres` service so the containers can authenticate with each other.

### 3. Launching the App

Ensure that `schema.sql` is in the same directory as your `docker-compose.yml` (the PostgreSQL container uses this to automatically build your tables on first boot).

Run the following command in your terminal to start the services in the background:

```bash
docker-compose up -d

```

To view the live application logs or troubleshoot connectivity:

```bash
docker-compose logs -f mashup_bot

```

To stop the application:

```bash
docker-compose down

```

---

## 🛠️ Architecture & Tech Stack

* **Language:** Python 3.13
* **Framework:** `discord.py` (v2.x)
* **Database:** PostgreSQL 15 (Alpine-optimized container)
* **Deployment:** Docker & Multi-container Docker Compose network

---

## 🗄️ Database Schema

The database initializes automatically using `schema.sql`. It isolates tracking metrics, tracks incoming stream indicators, and indexes search fields for high-performance link querying.

---

## 📝 Commands & Administration

* **`/sync`**: (Owner Only) Synchronizes application command trees globally with Discord API.
* The bot automatically monitors incoming messages in designated `MASHUP_CHANNEL_IDS` without requiring manual trigger prefixes.

```
