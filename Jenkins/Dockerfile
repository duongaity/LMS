FROM jenkins/jenkins:lts

# Switch to root user /highest priority
USER root

# Install third-party plugins required by Artemis
COPY plugins.yml /usr/share/jenkins/ref/plugins.yml
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.yml

# Update package list and install dependencies
RUN apt-get update -qq \
    && apt-get install -qqy \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg2 \
       software-properties-common

# Add Docker’s official GPG key / authenticate docker package
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -

# Add Docker repository
RUN echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
    > /etc/apt/sources.list.d/docker.list

# Update the package index and install Docker CE
RUN apt-get update -qq \
    && apt-get install -qqy docker-ce docker-ce-cli containerd.io

# Add Jenkins user to the Docker group
RUN usermod -aG docker jenkins

# Switch back to Jenkins user
USER jenkins
