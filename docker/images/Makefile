builder:

.PHONY:	builder-i386 builder-amd64 builder-arm64v8 custom-jenkins-docker all

all:
	make custom-jenkins-docker

builder-amd64:
	cd amd64 && docker build -t builder-amd64 .

builder-arm64v8:
	cd arm64v8 && docker build -t builder-arm64v8 .

builder-arm32v7:
	cd arm32v7 && docker build -t builder-arm32v7 .

custom-jenkins-docker:
	cd jendock && docker build -t custom-jenkins-docker .

builder-i386:
	cd i386 && docker build -t builder-i386 .

builder-ppc64le:
	cd ppc64le && docker build -t builder-ppc64le .

builder-s390x:
	cd s390x && docker build -t builder-s390x .
