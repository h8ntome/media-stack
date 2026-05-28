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
