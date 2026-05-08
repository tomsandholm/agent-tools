## Summary of AGENT-TOOLS

This is a collection of tools to support running jenkins in docker.  The jenkins folder has tools to connect to a running docker image for management.
The Makefile builds **Docker images** for Jenkins and multi-arch **builder agents**, and **runs operational helpers** against a running Jenkins container named `jenkins-controller`.

### Image builds

| Target | Purpose |
|--------|---------|
| **`all`** | Runs `custom-jenkins-docker` (default workflow). |
| **`builder`** | Empty target (placeholder). |
| **`custom-jenkins-docker`** | Start here first. Builds the Jenkins controller image from `jendock/`. |
| **`builder-amd64`**, **`builder-arm64v8`**, **`builder-arm32v7`**, **`builder-i386`**, **`builder-ppc64le`**, **`builder-s390x`** | Build agent images from matching subdirs; some use `--platform` for cross-arch builds. |

### Jenkins controller lifecycle

- **'docker/images/startup`** - This is the startup docker line to run jenkins.
- **`jenkins-start`** — Run `custom-jenkins-docker` detached with Docker socket, NFS mount at `/srv/nfs_share` → `/var/jenkins_home`, ports 8080 and 50000.
- **`jenkins-start-plugin-upgrade`** — Same idea but `PLUGINS_FORCE_UPGRADE=true` and a Docker volume `jenkins-vol` for home instead of NFS.
- **`jenkins-stop`** — Stop and remove `jenkins-controller`.
- **`jenkins-delete`** — Remove the `custom-jenkins-docker` image.

### Shell and secrets

- **`get-secret`** — Print Jenkins initial admin password from the host path.
- **`jenkins-shell`** / **`jenkins-shell-root`** — `docker exec` bash as `jenkins` or `root`.

### Automation / maintenance

- **`jenkins-get-cli`** — Fetch `jenkins-cli.jar` into the container if missing.
- **`jenkins-bulk-backup`** — `docker cp` full `/var/jenkins_home` to `/tmp/jenkins-full-backup/<timestamp>`.
- **`jenkins-push-token`** / **`jenkins-push-tools`** — Copy `token` and helper scripts into the container.
- **`jenkins-list-jobs`** / **`jenkins-list-plugins`** — Run list scripts inside the container (quiet recipes for piping to files).
- **`jenkins-update-plugins`** — Run `update-plugins` in the container.

### Mechanics

- **`.ONESHELL`** — Each recipe runs in one shell.
- **`DATE`** — Dynamic timestamp for backup directory naming.

Overall it is a small **ops + CI image** toolkit centered on building **`custom-jenkins-docker`** and **`builder-*`** images, then managing and scripting a long-lived Jenkins controller container.
