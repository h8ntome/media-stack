# Media Stack

A lightweight, fully automated home media server stack deployed using Docker Compose. This repository simplifies the process of spin-ups and tear-downs, providing a clean separation between application configuration and the execution layer.

---

## Quick Start (Complete Deployment)

### 1. Preparing a Fresh Server (Ubuntu/Debian)
If you are setting up on a brand-new server, you must install Docker and Docker Compose first. Copy and paste this block to prepare your system:

sudo apt update && sudo apt install -y docker.io docker-compose-plugin
sudo usermod -aG docker $USER && newgrp docker

### 2. One-Command Stack Deployment
Once Docker is installed, execute the following single-line command to clone this repository, enter the directory, and spin up all media services in the background:

git clone [https://github.com/h8ntome/media-stack.git](https://github.com/h8ntome/media-stack.git) && cd media-stack && docker compose up -d

---

## Services & Port Mapping

The stack isolates management, download, indexing, and streaming services. Below is a breakdown of the exposed interfaces:

- Jellyfin: Media streaming server & playback interface -> Port 8096
- qBittorrent: Torrent client (Web UI) -> Port 8082
- Prowlarr: Indexer manager -> Port 9696
- Radarr: Movie library automation -> Port 7878
- Sonarr: TV series library automation -> Port 8989
- Seerr: User request platform -> Port 5055
- Watchstate: Syncs play states -> Port 8001

---

## Data & Directory Architecture

Upon initial boot, Docker Compose maps local paths relative to the project directory:

media-stack/
├── .gitignore
├── README.md
├── docker-compose.yml
├── appdata/             <-- App databases, system preferences, metadata
└── /mnt/storage_pool/   <-- Requires host mounting for bulk media storage

Note: The configuration folders and media directories are explicitly omitted from Git tracking via the .gitignore file to protect your privacy.

---

## Post-Deployment Verification

Verify all containers are running cleanly:
docker compose ps

To tail live logs across all components:
docker compose logs -f

To stop the entire stack without deleting any app data:
docker compose down

---

## System Backups & Migrations

Because all application configurations are tied strictly to local host paths inside your appdata folder, full-stack data migration is completely seamless.

### Create a Complete Backup
To zip up and compress your entire configuration ecosystem, run:
tar -czvf media_stack_config_backup.tar.gz ./appdata

### Restore on a New Server
1. Complete the installation steps in the Quick Start section on your new server.
2. Drop your media_stack_config_backup.tar.gz file into the new server's media-stack/ directory.
3. Extract the contents: tar -xzvf media_stack_config_backup.tar.gz
4. Spin up the stack to instantly reload your data: docker compose up -d
