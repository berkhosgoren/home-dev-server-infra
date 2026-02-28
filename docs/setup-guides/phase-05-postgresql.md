# Phase 5: PostgreSQL (Docker) and LAN Network Refinement

This phase introduces a persistent PostgreSQL service running in Docker with cross-device access on the LAN. A single Postgres instance is used to host multipe project databases, while firewall rules are refined so Postgres is reachable only from the local subnet, keeping the service LAN-scoped and predictable.

## Goal

Run a stable PostgreSQL instance in Docker with persisten storage and LAN-only access for workstations and future backend containers.

## Completed Outcomes

* Docker-based PostgreSQL is running and survives restars using a persistent data directory on the storage disk.
* Workstation connectivity confirmed (GUI client connections validated).
* One Postgres instance is used to host multiple databases (one per project), rather than one container per project.
* Firewall rules refined so Postgres (5432/tcp) is allowed only from the LAN subnet (not globally exposed).
* A dedicated Docker network is used for future backend containers to connect to Postgres internally. 

## Key Details

* Service: PostgreSQL (containerized)
* Version: PostgreSQL 16 (image-based)
* Host: `homelab-server`
* LAN Access Example: `192.168.1.50:5432`
* Persistence: 
 
   * Postgres data is stored on the storage disk under a dedicated docker data directoryç
   * This keeps database state independent from the OS disk and supports rebuilds.

### Access Model

* LAN clients (workstations) connect using host IP + port:

  * Host: `192.168.1.50`
  * Port: `5432`
* Future backend containers connect using the Docker network hostname of the database service (container-to-container), not the host IP.

### Database Strategy (per-project)

This server uses a "one instance, many databases" approach:

* Create one database and one login role per project.
* Assign ownership of the project database to the project role.
* Connect from tools and applications using that project role.

**Example Pattern (names are illustrative):**

* Project Role: `project_user`
* Project Database: `project_db`
* Owner: `project_user`

### Basic Admin Workflow

Common tasks can be done either via a GUI client (recommended for day-to-day) or using `psql`.

**Inside a `psql` session:

* List databases: `\l`
* Switch database: `\c <database_name>`
* List roles/users: `\du`
* Exit: `\q`

### Application Connection Strings (example values)
Use the LAN host and port from any workstation or application running on the LAN:

* PostgreSQL:

  * `Host=192.168.1.50;Port=5432;Database=project_db;Username=project_user;Password=user_password`

**Note:** Store secrets securely (env vars, user-secrets, or a private config file), not in public documentation.

### Firewall / Exposure

Postgres is intentonally LAN-scoped:

* 5432/tcp is allowed from the local subnet.
* 5432/tcp is denied from "anywhere" as a default safety rule.

This keeps database access restricted to trusted local devices while still supporting cross-device development.

## Notes

* If compose reports "orphan containers" or container-name conflicts, clean-up is required before recreating services. This typically happens when a service name changes between compose edits.
* Docker Compose warnings about obsolete `version` fields can be addressed by rmeoving the `version:` key from the compose file when convenient.
