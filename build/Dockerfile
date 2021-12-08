FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

# Install pacakges
RUN apt update

# Install git, jdk, maven
RUN apt install -y git default-jdk maven

# Install docker 18.03
RUN apt -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
RUN apt update
RUN apt -y install docker-ce=18.03.1~ce-0~debian

# Clear cache
RUN apt clean
