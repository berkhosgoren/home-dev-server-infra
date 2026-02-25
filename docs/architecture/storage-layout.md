# Storage Layout

This file is for defining the storage architecture and mounting strategy, outlining how persistent data is organized and separated from the operating system and service workloads.

## Intent

Keep OS and persistent data seperate to support rebuilds and experimentation.

## Mount Strategy

* Persistent HDD mounted at `/srv/storage`
* UUID-based fstab entry
* `nofail` used to avoid boot failures if the disk is temporarily unavailable

## Seperation of Concerns

* User-facing shares: projects, docs, media, backups
* Service data: docker
* Future large assets: ai