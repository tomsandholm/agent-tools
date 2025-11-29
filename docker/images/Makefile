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

jenkins-stop:
	docker stop jenkins-controller
	docker rm jenkins-controller

jenkins-start:
	docker run \
		-u jenkins:jenkins \
		--name jenkins-controller \
		-d \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v jenkins-vol:/var/jenkins_home \
		-p 8080:8080 \
		-p 50000:50000 \
		--restart=on-failure custom-jenkins-docker

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

jenkins-shell:
	docker exec -it jenkins-controller bash

jenkins-shell-root:
	docker exec -u root -it jenkins-controller bash

jenkins-get-cli:
	docker exec -u root -it jenkins-controller bash -c 'cd /usr/share/jenkins && wget http://localhost:8080/jnlpJars/jenkins-cli.jar'

jenkins-bulk-backup:
	mkdir -p $(BACKUP_DIR)
	docker cp jenkins-controller:/var/jenkins_home $(BACKUP_DIR)

jenkins-list-jobs:
	make jenkins-get-cli
	docker cp token jenkins-controller:/usr/share/jenkins
	docker cp list-jobs jenkins-controller:/usr/local/bin
	docker exec -u root -it jenkins-controller bash -c '/usr/local/bin/list-jobs'

jenkins-list-plugins:
	#make -s jenkins-get-cli
	#docker cp -q --quiet token jenkins-controller:/usr/share/jenkins
	#docker cp -q --quiet list-plugins jenkins-controller:/usr/local/bin
	docker exec -u root -it jenkins-controller bash -c '/usr/local/bin/list-plugins' 2>/dev/null
