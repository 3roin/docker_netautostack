# Download base image
FROM jenkins/jnlp-agent-jdk11

# LABEL about the custom image
LABEL maintainer="3roin"
LABEL version="1.11.2"
LABEL description="jnlp-agent-ansible v1 jdk11 u2"

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Update Ubuntu Software repository and install packages
RUN echo "deb http://ftp.us.debian.org/debian buster main contrib non-free" > /etc/apt/sources.list && \
    apt-get update && \
    apt-get -qy full-upgrade && \
    apt-get install -qy git && \
    apt-get install openssh-server sudo -qy && \
# Install custom packages
    apt-get install -qy net-tools && \
    apt-get install -qy sshpass && \
    apt-get install -qy software-properties-common && \
#    apt-get install -qy ansible && \
    apt-get install python3 python3-pip -qy
# Install Ansible related packages
RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install ansible junos-eznc jxmlease  && \
    ansible-galaxy install juniper.junos --ignore-errors && \
    ansible-galaxy collection install  ansible.netcommon --ignore-errors && \
    ansible-galaxy collection install netbox.netbox --ignore-errors && \
# Create link for Ansible to use python3
    ln --symbolic /usr/bin/python3 /usr/bin/python
# Customize Ansible defaults
#    echo "[defaults]\nhost_key_checking = False\n[ssh_connection]\nscp_if_ssh=True" > /etc/ansible/ansible.cfg && \

# Create Ansible working directory and copy files
WORKDIR /home/ansible
COPY ansible-workdir /home/ansible

# Post-installation cleanup
RUN apt-get -qy autoremove && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

