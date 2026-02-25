# Phase 3B: Samba Network Shares

This phase enables secure network file sharing by exposing selected storage directories through Samba with authenticated SMB access. Only essential folders are shared to maintain seperation from internal service data, while cross-platform connectivity is ensured for both Windows and macOS clients using direct network paths for reliable access.

## Goal

Expose selected storage folders over SMB with required authentication.

## Shared Folders

* `projects`
* `docs`
* `media`
* `backups`

Not Shared:

* `docker`
* `ai`

## Authentication

SMB access requires login as an authenticated local user using a Samba password (mapped via `smbpasswd`).

## Client Access

### Windows

* `\\homelab-server`
* `\\192.168.1.50`

### macOS

* `smb://homelab-server.local`
* `smb://192.168.1.50`

## Notes
Windows network discovery can be inconsistent even when shares are fully functional.

Direct connect or mapping is reliable.