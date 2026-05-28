# Media Stack

A lightweight, fully automated home media server stack deployed using Docker Compose. This repository simplifies the process of spin-ups and tear-downs, providing a clean separation between application configuration and the execution layer.

---

## 🚀 Quick Start (Complete Deployment)

### 1. Preparing a Fresh Server (Ubuntu/Debian)
If you are setting up on a brand-new server, you must install Docker and Docker Compose first. Copy and paste this block to prepare your system:

```bash
# Update package lists and install Docker + Docker Compose Plugin
sudo apt update && sudo apt install -y docker.io docker-compose-plugin

# Give your current user permission to run Docker without 'sudo'
sudo usermod -aG docker $USER && newgrp docker
```

### 2. One-Command Stack Deployment
Once Docker is installed, execute the following single-line command to clone this repository, enter the directory, and spin up all media services in the background:

```bash
git clone https://github.com/h8ntome/media-stack.git && cd media-stack && docker compose up -d
```

---

## 📦 Services & Port Mapping

The stack isolates management, download, indexing, and streaming services. Below is a breakdown of the exposed interfaces:

| Service | Purpose | Port Mapping (Host -> Container) |
| :--- | :--- | :--- |
| **Jellyfin** | Media streaming server & playback interface | `8096:8096` |
| **qBittorrent** | Torrent client for media retrieval | `8082:8082` (Web UI)<br>`6881:6881` (TCP/UDP Data) |
| **Prowlarr** | Indexer manager for torrent trackers and Usenet | `9696:9696` |
| **Radarr** | Movie library management and automation | `7878:7878` |
| **Sonarr** | TV series library management and automation | `8989:8989` |
| **Seerr** | User-facing media request platform | `5055:5055` |
| **Watchstate** | Syncs play states/history across distinct media providers | `8001:8080` |

---

## 📂 Data & Directory Architecture

Upon initial boot, Docker Compose maps local bind-mount paths relative to the project directory. The root directory will generate the following folders dynamically:

```text
media-stack/
├── .gitignore
├── README.md
├── docker-compose.yml
├── appdata/             <-- App databases, system preferences, metadata
│   ├── jellyfin/
│   ├── prowlarr/
│   ├── qbittorrent/
│   ├── radarr/
│   ├── seerr/
│   ├── sonarr/
│   └── watchstate/
└── /mnt/storage_pool/   <-- *Requires host mounting for bulk media storage*
```


---

## ⚙️ Post-Deployment Verification

Verify all containers are running cleanly and view their health status:
```bash
docker compose ps
```

To tail live logs across all components for initial sync troubleshooting:
```bash
docker compose logs -f
```

To stop the entire stack without deleting any app data:
```bash
docker compose down
```

---

## 💾 System Backups & Migrations

Because all application configurations are tied strictly to local host paths inside your appdata folder, full-stack data migration is completely seamless.

### Create a Complete Backup
To zip up and compress your entire configuration ecosystem, run:
```bash
tar -czvf media_stack_config_backup.tar.gz ./appdata
```

### Restore on a New Server
1. Complete the installation steps in the **Quick Start** section on your new server.
2. Drop your `media_stack_config_backup.tar.gz` file into the new server's `media-stack/` directory.
3. Extract the contents:
   ```bash
   tar -xzvf media_stack_config_backup.tar.gz
   ```
4. Spin up the stack to instantly reload your users, history, and library structures:
   ```bash
   docker compose up -d
   ```
