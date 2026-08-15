# Docker Images Management

This directory contains the core infrastructure for building and managing a multi-architecture Jenkins environment using Docker. It includes Dockerfiles for various CPU architectures and a centralized `Makefile` to orchestrate builds, deployment, and administrative tasks.

## Makefile Summary

The `Makefile` serves as the primary entry point for:
1.  **Building Multi-Arch Images:** Creating Jenkins agent images for `amd64`, `arm64`, `armv7`, `i386`, `ppc64le`, and `s390x`.
2.  **Controller Management:** Building, starting, stopping, and upgrading the Jenkins controller image.
3.  **Administrative Utilities:** Automated backups, tool injection, plugin management, and shell access.

---

## Makefile Targets & Examples

### 1. Build Targets
Used to generate Docker images for the Jenkins controller and various agent architectures.

| Target | Description | Example |
| :--- | :--- | :--- |
| `all` | Default target; builds the controller image. | `make all` |
| `custom-jenkins-docker` | Builds the custom Jenkins controller image. | `make custom-jenkins-docker` |
| `builder-amd64` | Builds the `amd64` agent image. | `make builder-amd64` |
| `builder-arm64v8` | Builds the `arm64v8` agent image (requires `cc-tools`). | `make builder-arm64v8` |
| `builder-ppc64le` | Builds the `ppc64le` agent image. | `make builder-ppc64le` |

**Example: Building a specific agent**
```bash
# Build the ARM64v8 agent image
make builder-arm64v8
```

---

### 2. Container Lifecycle
Targets for managing the running Jenkins controller container.

| Target | Description | Example |
| :--- | :--- | :--- |
| `jenkins-start` | Starts the controller with NFS mounts and port mapping. | `make jenkins-start` |
| `jenkins-stop` | Stops and removes the `jenkins-controller` container. | `make jenkins-stop` |
| `jenkins-delete` | Removes the `custom-jenkins-docker` image. | `make jenkins-delete` |
| `jenkins-start-plugin-upgrade` | Starts Jenkins with forced plugin upgrades enabled. | `make jenkins-start-plugin-upgrade` |

**Example: Starting Jenkins with Persistence**
```bash
# Starts the container with /srv/nfs_share mounted to /var/jenkins_home
make jenkins-start
```

---

### 3. Administrative & Maintenance
Tools for interacting with the running Jenkins instance and performing maintenance.

| Target | Description | Example |
| :--- | :--- | :--- |
| `get-secret` | Retrieves the initial admin password from the host. | `make get-secret` |
| `jenkins-shell` | Opens a bash shell as the `jenkins` user. | `make jenkins-shell` |
| `jenkins-shell-root` | Opens a bash shell as the `root` user. | `make jenkins-shell-root` |
| `jenkins-bulk-backup` | Creates a timestamped backup of `jenkins_home`. | `make jenkins-bulk-backup` |
| `jenkins-push-tools` | Injects management scripts into the container. | `make jenkins-push-tools` |

**Example: Performing a Backup**
```bash
# Creates a backup in /tmp/jenkins-full-backup/MMDDYYYY-HH:MM:SS
make jenkins-bulk-backup
```

---

### 4. Plugin & Job Management
Targets for auditing and updating Jenkins resources via CLI.

| Target | Description | Example |
| :--- | :--- | :--- |
| `jenkins-list-plugins` | Lists installed plugins to stdout. | `make jenkins-list-plugins` |
| `jenkins-list-jobs` | Lists all Jenkins jobs. | `make jenkins-list-jobs` |
| `jenkins-update-plugins` | Runs the update script inside the container. | `make jenkins-update-plugins` |

**Example: Auditing Plugins**
```bash
# List all plugins and save to a file for tracking
make jenkins-list-plugins > installed_plugins.txt
```

---

## Directory Structure

- `agents/`: Jenkins Agent Dockerfiles grouped by architecture (amd64, arm, etc.).
- `controller/`: Dockerfile and configuration for the Jenkins controller.
- `applications/`: Standalone application images and multi-service setups.
  - `ansible/`: Ansible-specific Docker configuration.
  - `librebooking/`: Multi-service Librebooking setup (includes separate Makefile).
  - `spectcl/`: SpecTcl related images for Ubuntu 16.04 and Debian.
- `scripts/`: Administrative and management scripts pushed to the container or used for setup.
