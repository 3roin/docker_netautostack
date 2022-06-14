# Download base image ubuntu 20.04
FROM ubuntu:20.04

# LABEL about the custom image
LABEL maintainer="3roin"
LABEL version="0.1"
LABEL description="This is custom Docker Image for the Ansible"

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Create ansible user
RUN useradd -rm -d /home/ansible -s /bin/bash -g root -G sudo -u 1000 ansible && \
    echo ansible:ansible | chpasswd

# Update Ubuntu Software repository and install ssh
RUN apt update && \
    apt install openssh-server sudo -y && \
    service ssh start

# Install net-tools, ansible and related packages
RUN apt install -y net-tools && \
    apt install -y software-properties-common && \
    add-apt-repository --yes --update ppa:ansible/ansible && \
    apt install -y ansible && \
    ansible-galaxy install juniper.junos && \
    apt install -y python3-pip && \
    pip install junos-eznc && \
    pip install jxmlease

# Install git
RUN apt install -y git

# Post-installation cleanup
RUN rm -rf /var/lib/apt/lists/* && \
    apt clean

# Publish SSH port for the container
EXPOSE 22

# Default container command
#CMD ["bash"]
CMD ["/usr/sbin/sshd","-D"]
