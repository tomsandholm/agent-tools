# This Makefile is used to build the docker-images and to manage them
# Author: Thomas F. Sandholm
# Email: tom.sandholm@gmail.com
#
# Docker image builds
# all:  create the jenkins docker image
# builder-amd64 create the amd64 docker agent
# builder-arm64v8 creates the arm64v8 docker agent requires cc-tools
# builder-arm32v7 creates the arm32v7 docker agent requires cc-tools
# custom-jenkins-docker create the jenkins controller image
# get-secret returns the initial-jenkins-install password
# builder-i386 create the i386 docker agent requires cc-config
# build-ppc64le creates the ppc docker agent requires cc-tools
# builder-s390x creates the s390 docker agent requires cc-tools
# jenkins-stop stops the container and removes it
# jenkins-start starts the container
# jenkins-delete removes the docker image jenkins-controller
# jenkins-start-plugin-upgrade starts jenkins in force-plugin-update mode
# jenkins-shell opens shell connection to container as user jenkins
# jenkins-shell-root opens shell connection to container as user root
#

.ONESHELL:

DATE != date '+%m%d%Y-%H:%M:%S'
BACKUP_DIR := /tmp/jenkins-full-backup/$(DATE)

builder:

.PHONY:	builder-i386 builder-amd64 builder-arm64v8 custom-jenkins-docker all

all:
	make custom-jenkins-docker

builder-amd64:
	cd amd64 && docker build -t builder-amd64 .

builder-arm64v8:
	cd arm64v8 && docker build --platform linux/arm64 -t builder-arm64v8 . 

builder-arm32v7:
	cd arm32v7 && docker build -t builder-arm32v7 .

custom-jenkins-docker:
	cd jendock && docker build -t custom-jenkins-docker .

get-secret:
	sudo cat /var/lib/jenkins/secrets/initialAdminPassword

builder-i386:
	cd i386 && docker build -t builder-i386 .

builder-ppc64le:
	cd ppc64le && docker build -t builder-ppc64le --platform ppc64le .

builder-s390x:
	cd s390x && docker build --platform linux/s390x -t builder-s390x .

# stop the container
jenkins-stop:
	docker stop jenkins-controller
	docker rm jenkins-controller

# start the container
jenkins-start:
	docker run \
		-u jenkins:jenkins \
		--name jenkins-controller \
		-d \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v /srv/nfs_share:/var/jenkins_home \
		-p 8080:8080 \
		-p 50000:50000 \
		--restart=on-failure custom-jenkins-docker

# remove the custom-jenkins-dockre image
jenkins-delete:
	docker image rm custom-jenkins-docker

jenkins-start-plugin-upgrade:
	docker run \
		-e PLUGINS_FORCE_UPGRADE=true \
		-u jenkins:jenkins \
		--name jenkins-controller \
		-d \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v jenkins-vol:/var/jenkins_home \
		-p 8080:8080 \
		-p 50000:50000 \
		--restart=on-failure custom-jenkins-docker

# connect to container as user jenkins
jenkins-shell:
	docker exec -it jenkins-controller bash

# connect to container as root
jenkins-shell-root:
	docker exec -u root -it jenkins-controller bash

# populate container /usr/share/jenkins with the jenkins-cli.jar
jenkins-get-cli:
	@docker exec -u root -it jenkins-controller bash -c 'cd /usr/share/jenkins && [ -f jenkins-cli.jar ] || wget http://localhost:8080/jnlpJars/jenkins-cli.jar'

# run a full jenkins_home backup
jenkins-bulk-backup:
	mkdir -p $(BACKUP_DIR)
	docker cp jenkins-controller:/var/jenkins_home $(BACKUP_DIR)

# copy the authentication token to container in /usr/share/jenkins
jenkins-push-token:
	docker cp token jenkins-controller:/usr/share/jenkins

# copy the tools to docker container in /usr/local/bin
jenkins-push-tools:
	docker cp list-jobs jenkins-controller:/usr/local/bin
	docker cp list-plugins jenkins-controller:/usr/local/bin
	docker cp update-plugins jenkins-controller:/usr/local/bin
	docker cp list-plugins jenkins-controller:/usr/share/jenkins

# will return a list of jobs to stdout
# depends on toke and list-jobs script on docker-container
# supress the command echo so we can save to file without needing edits.
jenkins-list-jobs:
	@docker exec -u root -it jenkins-controller bash -c '/usr/local/bin/list-jobs'

# will return a list of plugins to stdout, no version data returned
# depends on token and list-plugins script on docker-container
# make this thing quiet so do not print the execution line, thus the @ in the command.  We want to save 
# directly to a file without needing any edits.
jenkins-list-plugins:
	@make --no-print-directory jenkins-get-cli
	@docker exec -u root -it jenkins-controller bash -c '/usr/local/bin/list-plugins' 2>/dev/null 

jenkins-update-plugins:
	docker exec -u root -it jenkins-controller bash -c '/usr/local/bin/update-plugins' 2>/dev/null 
