builder:

.PHONY:	builder-i386 builder-amd64 builder-arm64v8 custom-jenkins-docker all

all:
	make builder-i386
	make builder-amd64
	make builder-arm64v8
	make custom-jenkins-docker

builder-amd64:
	cd amd64 && docker build -t builder-amd64 .

builder-arm64v8:
	cd arm64v8 && docker build -t builder-arm64v8 .

custom-jenkins-docker:
	cd jendock && docker build -t custom-jenkins-docker .

builder-i386:
	cd i386 && docker build -t builder-i386 .
