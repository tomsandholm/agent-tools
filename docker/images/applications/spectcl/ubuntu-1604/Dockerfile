FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install -y build-essential default-jdk git autoconf automake libtool checkinstall sudo vim tcl tk
RUN useradd -rm -d /home/jenkins -s /bin/bash -G sudo jenkins -p "$(openssl passwd -1 jenkins)" 
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
COPY spectcl-5.14-008-bullseye.tar.gz /root/

