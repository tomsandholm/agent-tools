FROM i386/ubuntu
RUN apt-get update
RUN apt-get install -y openssh-server build-essential default-jdk git autoconf automake libtool checkinstall sudo vim
RUN useradd -rm -d /home/jenkins -s /bin/bash -g root -G sudo jenkins -p "$(openssl passwd -1 jenkins)"
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN service ssh start
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
