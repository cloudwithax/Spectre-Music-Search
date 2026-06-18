# Spectre Music Search Bot 🎵

A production-ready, containerized Discord bot built with Python (`discord.py`) and PostgreSQL. It monitors specified text channels for music links, parses streaming and YouTube metadata, and maintains a persistent database ledger of your community's track mashups and shared music.

---

## 🚀 Quick Start (Docker Deployment)

This project is fully containerized using Docker Compose, allowing you to deploy the bot and its database with a single command.

### 1. Prerequisites
Make sure you have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your host machine.

### 2. Configuration
Clone this repository to your server, then open the `docker-compose.yml` file. You will need to configure the environment blocks for **both** services. 

Here is how the environment variables should look:

```yaml
services:
  mashup_bot:
    # ... (other configuration) ...
    environment:
      - DISCORD_TOKEN=your_bot_token_here          # From Discord Developer Portal
      - OWNER_DISCORD_IDS=123456789,987654321      # Comma-separated admin user IDs
      - MASHUP_CHANNEL_IDS=1122334455,6677889900   # Comma-separated channel IDs to monitor
      
      # Database connection settings (Must match postgres block below)
      - DB_HOST=mashups_postgres                    # Keep this as the service name
      - DB_NAME=mashups_db                          # 1. Must match POSTGRES_DB
      - DB_USER=postgres_user                      # 2. Must match POSTGRES_USER
      - DB_PASSWORD=your_secure_password           # 3. Must match POSTGRES_PASSWORD

  mashups_postgres:
    # ... (other configuration) ...
    environment:
      - POSTGRES_DB=mashups_db                      # 1. Custom database name
      - POSTGRES_USER=postgres_user                # 2. Custom database admin user
      - POSTGRES_PASSWORD=your_secure_password     # 3. Secure database password

```

> ⚠️ **Important Security Rule:** The values for **DB_NAME**, **DB_USER**, and **DB_PASSWORD** under `mashup_bot` **must perfectly match** the **POSTGRES_DB**, **POSTGRES_USER**, and **POSTGRES_PASSWORD** values under `mashup_postgres`. If they mismatch, the bot will fail to authenticate with the database and crash on boot.

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

The database initializes automatically using `schema.sql` upon the container's very first launch. It isolates tracking metrics, tracks incoming stream indicators, and indexes search fields for high-performance link querying.

---

## 📝 Commands & Administration

* **`/sync`**: (Owner Only) Synchronizes application command trees globally with Discord API.
* **`/search keyword: `**: (everyone) Searches for mashups that include the keyword or similar.
* The bot automatically monitors incoming messages in designated `MASHUP_CHANNEL_IDS` without requiring manual trigger prefixes.
