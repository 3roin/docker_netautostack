# Download base image
FROM jenkins/jnlp-agent-jdk11

# LABEL about the custom image
LABEL maintainer="3roin"
LABEL version="1.0.0"
LABEL description="jnlp-agent-jdk11-ansible"

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Update Ubuntu Software repository and install packages
RUN apt-get update && \
    apt-get -qy full-upgrade && \
    apt-get install -qy git && \
    apt-get install openssh-server sudo -qy && \
# Install net-tools, ansible and related packages
    apt-get install -qy net-tools && \
    apt-get install -qy software-properties-common && \
#    add-apt-repository --yes --update ppa:ansible/ansible && \
    apt-get install -qy ansible && \
    ansible-galaxy install juniper.junos && \
    apt-get install -qy python3-pip && \
    pip3 install junos-eznc && \
    pip3 install jxmlease

# Post-installation cleanup
RUN apt-get -qy autoremove && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

