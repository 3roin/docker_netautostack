# Download base image ubuntu 20.04
FROM ubuntu:20.04

# LABEL about the custom image
LABEL maintainer="3roin"
LABEL version="0.1"
LABEL description="Ansible-Jenkins-Slave"

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Create custom user and password (you may want to skip this step)
#RUN useradd -rm -d /home/ansible -s /bin/bash -g root -G sudo -u 1000 ansible && \
#    echo ansible:ansible | chpasswd
RUN group add -g 1001:jenkins
    useradd -rm -d /home/jenkins -s /bin/bash -g jenkins -G sudo -u 1001 jenkins && \
    echo jenkins:jenkins | chpasswd && \
    mkdir /home/jenkins/.m2 &&\
    mkdir /home/jenkins/.ssh &&\
    chown -R jenkins:jenkins /home/jenkins/.m2/ && \
    chown -R jenkins:jenkins /home/jenkins/.ssh/

# Update Ubuntu Software repository and install ssh
RUN apt-get update && \
    apt-get -qy full-upgrade && \
    apt-get install -qy git && \
    apt-get install openssh-server sudo -qy && \
    service ssh start

# Install net-tools, ansible and related packages
RUN apt-get install -qy net-tools && \
    apt-get install -qy software-properties-common && \
    add-apt-repository --yes --update ppa:ansible/ansible && \
    apt-get install -qy ansible && \
    ansible-galaxy install juniper.junos && \
    apt-get install -qy python3-pip && \
    pip install junos-eznc && \
    pip install jxmlease

# Install git
RUN apt-get install -qy git && \
# Install JDK 11
    apt-get install -qy openjdk-11-jdk && \
# Install maven
    apt-get install -qy maven

# Post-installation cleanup
RUN apt-get -qy autoremove && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

# Download Jenkins plugins
RUN jenkins-plugin-cli --plugins "ansible:1.1 swarm:3.33"

# Copy authorized keys
#COPY .ssh/authorized_keys /home/jenkins/.ssh/authorized_keys

# Publish SSH port for the container
EXPOSE 22

# Auto-Join Jenkins Cluster using Swarm plugin (source: https://plugins.jenkins.io/swarm/)
#RUN wget ${JENKINS_URL}/swarm/swarm-client.jar && \
#    java -jar ${path/to/swarm-client.jar} -url ${JENKINS_URL} -username ${USERNAME} -password ${PASSWORD}

# Default container command
CMD ["/usr/sbin/sshd","-D"]
