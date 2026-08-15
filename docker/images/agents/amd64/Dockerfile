FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y openssh-server build-essential default-jdk git autoconf automake libtool checkinstall sudo vim nfs-common
RUN deluser --remove-home ubuntu
RUN groupadd -g 1000 jenkins
RUN useradd -u 1000 -g 1000 -rm -d /home/jenkins -s /bin/bash -G sudo jenkins -p "$(openssl passwd -1 jenkins)" 
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo "10.0.0.150:/srv/nfs_share /home/jenkins defaults 0 0" >> /etc/fstab
RUN service ssh start
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
