FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y build-essential openssh-server default-jdk git autoconf automake libtool checkinstall sudo vim
RUN useradd -rm -d /home/ansible -s /bin/bash -G sudo ansible -p "$(openssl passwd -1 ansible)" 
RUN echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN mkdir -p /home/ansible/.ssh && chmod 0700 /home/ansible/.ssh
COPY id_rsa.pub /home/ansible/.ssh/id_rsa.pub
COPY id_rsa /home/ansible/.ssh/id_rsa
COPY authorized_keys /home/ansible/.ssh/authorized_keys
RUN chown -R ansible:ansible /home/ansible/.ssh && \
  chmod 0600 /home/ansible/.ssh/*
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

