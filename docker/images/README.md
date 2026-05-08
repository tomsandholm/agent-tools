# Docker Images Management

This repository contains Dockerfiles and a Makefile to build and manage Jenkins controller and agent images for various architectures.

## Overview

The project is organized to support multi-architecture builds for Jenkins agents and a customized Jenkins controller image. The root `Makefile` provides a comprehensive set of commands for building images, managing containers, and performing administrative tasks.

## Makefile Targets

### Build Targets

- `make all`: Default target, builds the custom Jenkins controller image (`custom-jenkins-docker`).
- `make custom-jenkins-docker`: Builds the Jenkins controller image from the `jendock/` directory.
- `make builder-amd64`: Builds the Jenkins agent image for the `amd64` architecture.
- `make builder-arm64v8`: Builds the Jenkins agent image for the `arm64v8` architecture.
- `make builder-arm32v7`: Builds the Jenkins agent image for the `arm32v7` architecture.
- `make builder-i386`: Builds the Jenkins agent image for the `i386` architecture.
- `make builder-ppc64le`: Builds the Jenkins agent image for the `ppc64le` architecture.
- `make builder-s390x`: Builds the Jenkins agent image for the `s390x` architecture.

### Management Targets

- `make jenkins-start`: Starts the Jenkins controller container (`jenkins-controller`) in the background. It mounts `/var/run/docker.sock` and a local NFS share `/srv/nfs_share` to `/var/jenkins_home`.
- `make jenkins-stop`: Stops and removes the `jenkins-controller` container.
- `make jenkins-delete`: Deletes the `custom-jenkins-docker` image.
- `make jenkins-start-plugin-upgrade`: Starts the Jenkins controller with the `PLUGINS_FORCE_UPGRADE=true` environment variable enabled.
- `make jenkins-shell`: Opens an interactive bash shell in the running Jenkins container as the `jenkins` user.
- `make jenkins-shell-root`: Opens an interactive bash shell in the running Jenkins container as the `root` user.

### Administrative & Utility Targets

- `make get-secret`: Displays the initial Jenkins admin password from the host system.
- `make jenkins-get-cli`: Downloads the `jenkins-cli.jar` into the container if it doesn't already exist.
- `make jenkins-bulk-backup`: Creates a timestamped backup of the `/var/jenkins_home` directory from the container to `/tmp/jenkins-full-backup/`.
- `make jenkins-push-token`: Copies a `token` file from the host to the container's `/usr/share/jenkins` directory.
- `make jenkins-push-tools`: Copies utility scripts (`list-jobs`, `list-plugins`, `update-plugins`) from the root directory to the container's `/usr/local/bin`.
- `make jenkins-list-jobs`: Lists Jenkins jobs using the `list-jobs` script inside the container.
- `make jenkins-list-plugins`: Lists installed Jenkins plugins.
- `make jenkins-update-plugins`: Executes the `update-plugins` script inside the container.

## Multi-Architecture Support

The repository includes subdirectories for each supported architecture (e.g., `amd64/`, `arm64v8/`, etc.), each containing its own `Dockerfile`.

## Librebooking (Multi-Service)

The `multi/` directory contains a separate setup for "Librebooking", which includes its own `Makefile` and `docker-compose.yml` for managing a database and PHP application suite.
