# Phase 4: Container Runtime Foundation

This phase introduces the container runtime layer that will host future services and development workloads. Docker is configured as the standardized execution environment, with persistent storage relocated to the dedicated data disk to keep containers isolated from the operating system. Establishing this foundation prepares the system for database stacks, backend services, and future GPU workloads while maintaining a clean and reproducible infrastructure layout.

## Goal

Establish a stable container runtime environment with persistent storage separation and a predictable directory structure for future stacks.

## Completed Outcomes

* Docker Engine installed and running as a system service.
* Non-root container execution enabled for remote administration workflows.
* Docker data root relocated to the storage volume to prevent OS disk growth.
* Base container stack directory created under `/srv/storage/docker-stacks`.
* Logical separation introduced for infrastructure, applications, and templates.

## Key Details

### Example:
* Hostname: `homelab-server`
* Container runtime: Docker Engine
* Data root location: `/srv/storage/docker`
* Stack directory:
   ```bash
   /srv/storage/docker-stacks/
   ├── apps/
   ├── infra/
   ├── postgres/
   ├── mssql/
   └── templates/
    ```

### Design Choices Made in This Phase:

* Containers do not store persisten data on the system disk.
* Storage volume acts as the single source of truth for container state.
* Directory layout is prepared early to prevent ad-hoc stack placement later.

## Notes

This phase intentionally avoids deploying any production services.

The objective is to stabilize the runtime layer before introducing databases or APIs.

Future phases will build on this foundation:

* Database stacks (PostgreSQL / SQL Server)
* Reverse proxy and HTTPS routing
* GPU-enabled AI workloads
* Monitoring and backup automation