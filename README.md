\# 🏠 Home Dev Server Infrastructure

> Personal self-hosted development server focused on backend work, automation, and AI experimentation.



This repository documents the setup and evolution of my personal home development server.

My goal is to maintain a stable self-hosted environment for backend development, experimentation, and AI 

workloads while keeping everything reproducible and easy to rebuild if something breaks.



This server is designed to act as a multi-purpose home lab - part infrastructure playground - part real

production environment for personal projects.



---



\## 🚀 Phase 1 — Base Server Setup

Phase 1 focuses on getting a reliable foundation running: clean OS install, predictable networking, and

remote access. Nothing fancy yet -- just a solid base that future automation and services can build on.



---



\### 💻 Hardware Layout

Current storage configuration separates system reliability from data storage:

\- \*\*SSD (224 GB)\*\*

&nbsp; Hosts the operating system and core services. Keeping the OS isolated makes upgrades, reinstalls, and 

&nbsp;  backups safer.

\- \*\*HDD (1TB)\*\*

&nbsp; Reserved for persistent data:

&nbsp; - databases

&nbsp; - shared files

&nbsp; - container volumes

&nbsp; - experiment datasets



This split also makes it easier to replace the OS without touching long-term storage.



---



\### 🐧 Operating System



\- Ubuntu Server 24.04 LTS

\- Hostname: `devserver`

\- OpenSSH installed during setup for immediate remote access



I chose Ubuntu LTS because it's predictable, well-documented, and boring in a good way -- stability matters

more than novelty on infrastructure machines.



---



\### 🌐 Networking

The server runs on a static LAN address so services don't randomly disappear when DHCP changes:

\- Interface: `enp37s0`

\- Static IP: `192.168.1.102`

\- Gateway: `192.168.1.1`



Static addressing keeps reverse proxies, SSH configs, and network shares consistent across the home network.



---



\### 🔐 Access \& Administration

All management happens remotely from my main workstation:

\- SSH is the primary control channel

\- No desktop environment installed -- everything stays lightweight and scriptable

\- Future plans include tightening access with key-only auth and possible a VPN entry point



---



\### 🧩 Intended Server Roles

This machine is not a single-purpose server, it's meant to grow into a small self-hosted platform.



Planned responsibilities include:

\- 🗄️ Database Hosting

&nbsp; Local databases for development and testing environments.

\- ⚙️ Backend Services

&nbsp; APIs, microservices, and experimental projects.

\- 📂 Network Storage (SMB)

&nbsp; Shared folders accessible from my other devices on the network.

\- 🐳 Docker Workloads

&nbsp; Containers for isolated services and reproducible setups.

\- 🤖 AI Workloads (GPU-enabled)

&nbsp; Model experimentation, local inference, and automation tools.



---



\### 📈 Long-Term Direction

This repo will gradually evolve beyond simple notes into a living infrastructure reference.

I plan to document every step, decision, and mistake as this system evolves.



Future phases will likely include: 

\- Automated provisioning scripts

\- Docker Compose stacks

\- Monitoring \& Logging

\- Backup strategies

\- GPU runtime configuration

\- Reverse proxy + HTTPS setup



My goal is to document decisions as they happen -- including mistakes -- so rebuilding the system later

doesn't require me to guess what I was trying to implement at that time. 



---



\### 📝 Philosophy

This is intentionally a practical home lab, not an enterprise-grade setup.



Some guiding principles:

\- Keep it understandable at 2 AM when something breaks

\- Prefer boring, stable tools over trendy ones

\- Document changes even when they seem obvious at the time



--- 







