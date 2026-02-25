# 🏠 Home Dev Server Infrastructure

> Personal self-hosted development server focused on backend work, automation, and AI experimentation.

## ⚙️ Overview

| Category | Details |
|---|---|
| OS | Ubuntu Server 24.04 LTS |
| Role | Self-hosted Dev & AI Lab |
| Storage Layout | SSD (OS) + HDD (Persistent Data) |
| Access | Headless via SSH |
| Documentation | `/docs/setup-guides/` |

---

> ⚠️ **Note:** Hostnames, IP addresses, usernames, and disk names shown in this repository are example values used for documentation purposes. 

## 🧭 Project Overview

My goal is to maintain a stable self-hosted environment for backend development, experimentation, and AI workloads while keeping everything reproducible and easy to rebuild if something breaks.

This server is designed as a multi-purpose home lab — part infrastructure playground, part real production environment for personal projects.


## 📚 Setup Documentation

Detailed setup and configuration phases are documented under:

* `/docs/setup-guides/phase-01-base-setup.md`
* `/docs/setup-guides/phase-02-remote-access-networking.md`
* `/docs/setup-guides/phase-03a-storage-setup.md`
* `/docs/setup-guides/phase-03b-samba-network-shares.md`

## 💻 Hardware Layout

Current storage configuration separates system reliability from data storage:

- **SSD (224 GB)**
  - Hosts the operating system and core services. Keeping the OS isolated makes upgrades, reinstalls, and backups safer.

- **HDD (1TB)**
  - Reserved for persistent data:
    - databases
    - shared files
    - container volumes
    - experiment datasets

This split also makes it easier to replace the OS without touching long-term storage.

## 🐧 Operating System
 - Ubuntu Server 24.04 LTS
 - Hostname: `homelab-server` (example) 
 - OpenSSH installed during setup for immediate remote access

I chose Ubuntu LTS because it's predictable, well-documented, and boring in a good way -- stability matters more than novelty on infrastructure machines.

## 🌐 Networking
The server runs on a static LAN address so services don't randomly disappear when DHCP changes:
 - Interface: `enp37s0`
 - Static IP: `192.168.1.50` (example)
 - Gateway: `192.168.1.1` (example)

Static addressing keeps reverse proxies, SSH configs, and network shares consistent across the home network.

## 🔐 Access & Administration
All management happens remotely from my main workstation:
 - SSH is the primary control channel
 - No desktop environment installed -- everything stays lightweight and scriptable
 - Future plans include tightening access with key-only auth and possible a VPN entry point

## 🧩 Intended Server Roles
This machine is not a single-purpose server, it's meant to grow into a small self-hosted platform.

Planned responsibilities include:
 - 🗄️ Database Hosting
   - Local databases for development and testing environments.
 - ⚙️ Backend Services
   - APIs, microservices, and experimental projects.
 - 📂 Network Storage (SMB)
   - Shared folders accessible from my other devices on the network.
 - 🐳 Docker Workloads
   - Containers for isolated services and reproducible setups.
 - 🤖 AI Workloads (GPU-enabled)
   - Model experimentation, local inference, and automation tools.

## 📈 Long-Term Direction
This repo will gradually evolve beyond simple notes into a living infrastructure reference.

I plan to document every step, decision, and mistake as this system evolves.

Future phases will likely include: 
 - Automated provisioning scripts
 - Docker Compose stacks
 - Monitoring & Logging
 - Backup strategies
 - GPU runtime configuration
 - Reverse proxy + HTTPS setup


My goal is to document decisions as they happen -- including mistakes -- so rebuilding the system later
doesn't require me to guess what I was trying to implement at that time. 

## 📝 Philosophy
This is intentionally a practical home lab, not an enterprise-grade setup.

Some guiding principles:
 - Keep it understandable at 2 AM when something breaks
 - Prefer boring, stable tools over trendy ones
 - Document changes even when they seem obvious at the time








