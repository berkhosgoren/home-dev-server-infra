# Phase 3A: Storage Setup

This phase prepares dedicated long-term storage by mounting the 1TB HDD at `/srv/storage` and organizing a clear directory layout for projects, services, and backups. Using an ext4 filesystem with UUID-based mounting ensures stable persistence across reboots while seperating operating system files from user data.

## Goal

Configure the 1TB HDD as persistent storage mounted at `/srv/storage` and prepare a structured layout for shares and services.

## Disk Layout

* SSD: OS and core services
* HDD: persistent data

## Mount 

* Disk: `<data-disk>` 
* Mount point: `/srv/storage`
* Filesystem: `ext4`
* Persistent mount: `/etc/fstab` using UUID

## Folder Structure 

```bash
/srv/storage/
├── projects/
├── media/
├── docs/
├── backups/
├── docker/
└── ai/
```

## Notes

Mount uses UUID rather than `/dev/sdb1` to prevent breakage if device enumeration changes after reboot. 