# 🐳 Ultimate Docker-Traefik Repo by Andrew Lush

> **Real-world Docker configurations** from Andrew at [SimpleHomelab.com](https://www.simplehomelab.com/) (formerly SmartHomeBeginner.com)

[![UDMS Series](https://img.shields.io/badge/UDMS-Series-green?style=flat-square)](https://www.simplehomelab.com/ultimate-docker-media-server-udms-01/)
[![150+ Apps](https://img.shields.io/badge/Apps-150%2B-orange?style=flat-square)](#-featured-applications)

---

## 🎯 What This Repository Offers

This repository contains my **actual production Docker setups** that power my homelab infrastructure. Unlike theoretical guides, these are real-world configurations that I use daily.

My setup is based on [Ultimate Docker Media Server](https://www.simplehomelab.com/ultimate-docker-media-server-udms-01/) series. 

### 🎯 Repository Purpose
1. **Share actual setups** - Real configurations I use in production
---

## 🖥️ Infrastructure Overview

I believe in **simple, energy-efficient homelab** design that maximizes performance while minimizing complexity.

### 🏠 Networking Architecture
- **OPNsense** home firewall running on Proxmox VM (DMZed on ISP router)
- **Tailscale** mesh networking connecting all hosts

### 📊 Hardware Specifications

| Component | Specifications | Purpose |
|-----------|---------------|---------|
| **TopTon V700 Mini PC** | Intel i7-13800H, 64GB RAM, 2×2TB NVMe ZFS RAID1, 4TB SATA SSD | Proxmox host that runs my Home Server, Media/Database Server, Home Assistant, Proxmox Backup Server, etc. |
| **Synology DS918+** | DX517 Expansion Unit, 8GB RAM, 4×18TB SHR2 (×2 volumes) | Primary use is storage. But I tinker with Docker on it. |
| **Oracle Ampere A1** | 4 vCPU ARM64, 24GB RAM, 200GB storage | Web server and automations |

---

## 🐳 Docker Hosts

All Docker configurations are organized by host with clear naming conventions. I pool them together and push them to this repository. 

### 🏠 Home Server (prefix/suffix: `hs`)
```yaml
Platform: Ubuntu 24.04 LXC on Proxmox
Resources: 8 vCPU, 8GB RAM, 4GB Swap
Storage: 64GB OS + 32GB Docker data
Purpose: Core homelab services and support my tinkering habits
```

### 🎬 Media Database Server (prefix/suffix: `mds`)
```yaml
Platform: Ubuntu 24.04 LXC on Proxmox  
Resources: 12 vCPU, 12GB RAM, 4GB Swap
Storage: 64GB OS + 72GB Docker data
Purpose: Media servers and databases - Separate so they are not affected by my tinkering
```

### 🌐 Web Server (prefix/suffix: `ws-arm`)
```yaml
Platform: Ubuntu 24.04 ARM64 on Oracle Cloud
Resources: 4 vCPU, 24GB RAM
Storage: 100GB OS + 100GB data
Purpose: Web Server (Nginx, PHP-FPM 8, WordPress, etc.), n8n, Flowise, and more
```

### 💾 Synology NAS (prefix/suffix: `ds918`)
```yaml
Platform: DSM 7.X
Resources: 8GB RAM, DX517 Expansion Unit, Volume1 - 4x18TB SHR2, Volume 2 - 4x18TB SHR2
Purpose: I use this only for tinkering with Docker
```
---

## 📚 Learning Resources

### 🎥 Ultimate Docker Media Server Guides and Videos

#### 🚀 Getting Started
1. [Introduction and Overview](https://www.simplehomelab.com/udms-01-introduction-and-overview/)
2. [Hardware: NAS, Mini PC, or VPS (FREE!). Which one?](https://www.simplehomelab.com/udms-02-hardware-nas-minipc-vps/)
3. [Best Home Server OS, Proxmox LXC vs VM](https://www.simplehomelab.com/udms-03-best-home-server-os/)

#### ⚙️ Infrastructure Setup
4. [Install Proxmox on Mini PC with ZFS RAID1 Mirror + 3 Tweaks](https://www.simplehomelab.com/udms-04-install-proxmox-on-mini-pc/) [📹](https://youtu.be/2nIPY7D-UA0)
5. [Installing and Prepping Ubuntu/Debian](https://www.simplehomelab.com/udms-05-installing-ubuntu-on-proxmox/) [📹](https://youtu.be/-ZSQdJ62r-Q)
6. [Mounting Remote Folders using Rclone](https://youtu.be/D-XS0biLP14) [📹](https://youtu.be/D-XS0biLP14)
7. Mounting Remote Folders using SMB/CIFS *(Coming Soon)*
8. Mounting Remote Folders using NFS *(Coming Soon)*
9. Binding Mounting on Proxmox Unprivileged LXC *(Coming Soon)*

#### 🔧 Advanced Configuration
10. [Proxmox Unprivileged LXC Network Node Passthrough](https://www.simplehomelab.com/udms-10-proxmox-lxc-network-device-passthrough/) [📹](https://youtu.be/r0nGMFs5pCY)
11. [Proxmox Unprivileged LXC iGPU Node Passthrough](https://www.simplehomelab.com/udms-11-gpu-passthrough-on-proxmox-lxc/) [📹](https://youtu.be/kvnJYyyLoIk)

#### 🐳 Docker Fundamentals
12. [Installing Docker and Docker Compose on Ubuntu/Debian](https://www.simplehomelab.com/udms-12-install-docker-and-docker-compose/)
13. [Essential Docker Commands & Time-Saving Aliases](https://www.simplehomelab.com/udms-13-docker-and-docker-compose-commands/)
14. [Kickass Docker Media Server with 150+ Apps](https://www.simplehomelab.com/udms-14-docker-media-server/) [📹](https://youtu.be/THuLgGwq0vg)
15. Best Docker Containers for Homelab *(Coming Soon)*

#### 🌐 Remote Access & Security
16. [Exposing Apps to the Internet: Tailscale](https://www.simplehomelab.com/udms-part-16-tailscale-homelab-remote-access/) [📹](https://youtu.be/M6GMp4FJrB8)
17. Exposing Apps to the Internet: Nginx Proxy Manager *(Coming Soon)*
18. [Exposing Apps to the Internet: Traefik Reverse Proxy](https://www.simplehomelab.com/udms-18-traefik-docker-compose-guide/) [📹](https://www.youtube.com/playlist?list=PL1Hno7tIbSWUGrZSqeB9aCsdAuoeVwvgh)

#### 🔐 Authentication & Security
19. [Authentication for Docker Apps - Authelia](https://www.simplehomelab.com/udms-19-authelia-docker-compose/) [📹](https://youtu.be/UIq8PLZHBtk)
20. [Authentication for Docker Apps - Google OAuth 2](https://youtu.be/SCKALXprTQE) [📹](https://youtu.be/SCKALXprTQE)
21. [Authentication for Docker Apps - Authentik](https://youtu.be/GoUmJAe1MKc) [📹](https://youtu.be/GoUmJAe1MKc)
22. [CrowdSec Docker Compose – Bulletproof IPS for Homelabs](https://www.simplehomelab.com/udms-22-crowdsec-docker-compose/)
23. [Setting up Crowdsec Cloudflare Bouncer](https://www.simplehomelab.com/udms-23-crowdsec-cloudflare-bouncer/)
24. [Setting up Crowdsec Traefik Bouncer](https://www.simplehomelab.com/udms-24-crowdsec-traefik-bouncer/)

#### 🚀 Advanced Topics
25. Advanced Topics: Traefik Plugins *(Coming Soon)*
26. Advanced Topics: Traefik Multiple Domains *(Coming Soon)*
27. Advanced Topics: Traefik Domain Passthrough *(Coming Soon)*
28. Advanced Topics: [Traefik Conditional Auth Bypass](https://www.simplehomelab.com/udms-28-traefik-auth-bypass/)
29. Advanced Topics: [CrowdSec Multiserver Setup](https://www.simplehomelab.com/udms-29-crowdsec-multiserver/)
30. Closing Thoughts and Options to Level Up *(Coming Soon)*

---

## 🚀 Featured Applications

**150+ Docker applications** ready for deployment, sourced from the [Deployrr Repository](https://github.com/SimpleHomelab/Deployrr/blob/main/APPS.md):

Adminer, Airsonic-Advanced, Authentik, Audiobookshelf, Authelia, Baikal, Bazarr, Beets, Bookstack, cAdvisor, Calibre, Calibre-Web, Change Detection, Chromium, Cleanuparr, Cloud Commander, Cloudflare Tunnel, CrowdSec, CrowdSec Firewall Bouncer, CyberChef, Dashy, DDNS Updater, DeUnhealth, DigiKam, Dockwatch, Docker Garbage Collection, DokuWiki, Double Commander, Dozzle, Dozzle Agent, DweebUI, Emby, ESPHome, FileZilla, Flame, Flaresolverr, Flowise, FreshRSS, Funkwhale, GameVault, Glances, Gluetun, Gonic, Gotenberg, GPTWOL, Grafana, Grocy, Guacamole, Heimdall, Homarr, Home Assistant Core, Homebridge, Homer, Homepage, Huntarr, Immich, InfluxDB, IT-Tools, Jackett, Jellyfin, Jellyseerr, Kasm, Kavita, Kometa, Komga, Lidarr, Lollypop, Maintainerr, MariaDB, Mosquitto, MQTTX Web, Mylar3, n8n, Navidrome, Netdata, Nextcloud, Node Exporter, Node-RED, Notifiarr, OAuth, Ollama, Ombi, OpenHands, Open-WebUI, Organizr, Overseerr, Paperless-AI, Paperless-NGX, PdfDing, PgAdmin, phpMyAdmin, Pi-hole, Piwigo, Plex, Portainer, PostgreSQL, Privatebin, Prometheus, Prowlarr, qBittorrent, qBittorrent with VPN, Qdrant, Radarr, Redis, Redis Commander, Remmina, Resilio Sync, SABnzbd, Scrutiny, SearXNG, ShellInABox, Smokeping, Socket Proxy, Sonarr, Speedtest-Tracker, SSHwifty, Stirling PDF, Tailscale, Tautulli, The Lounge, Theme Park, Tika, TinyAuth, Traefik, Traefik Access Logs, Traefik Bouncer, Traefik Certs Dumper, Traefik Error Logs, Transmission, Trilium Next, Uptime-Kuma, Vaultwarden, Vikunja, Visual Studio Code Server, Wallos, Watchtower, Weaviate, WG-Easy, What's Up Docker (WUD), WikiDocs, Wireguard, and ZeroTier.

---

## ⚡ Quick Start Commands

### 🎯 Essential Docker Aliases

I use **Bash Aliases** installed via Deployrr for streamlined Docker management:

| Command | Description |
|---------|-------------|
| `dcup` | Start Docker stack |
| `dcdown` | Stop Docker stack |
| `dcrec` | Start or recreate specific service/full stack |
| `dcstop` | Stop specific service/full stack |
| `dcrestart` | Restart specific service/full stack |
| `dclogs` | View real-time logs for stack/service |
| `dcpull` | Pull new images for stack/service |

> 📖 **Learn More**: [Essential Docker Commands & Time-Saving Aliases](https://www.simplehomelab.com/udms-13-docker-and-docker-compose-commands/) | [Bash Aliases in Deployrr](https://docs.deployrr.app/operating-system/bash-aliases-explained)

---

