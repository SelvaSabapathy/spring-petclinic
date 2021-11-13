FROM jenkins/jenkins:lts-jdk11

USER root

# install docker client
RUN apt-get update
RUN apt-get install -y build-essential
RUN curl -sSL https://get.docker.com/ | sh

# docker inside docker issue resolution
# https://stackoverflow.com/questions/48568172/docker-sock-permission-denied
RUN usermod -aG docker jenkins

USER jenkins

# disable web interface
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

# add local security relam
ENV CASC_JENKINS_CONFIG /var/jenkins_home/casc.yml
COPY ./casc.yml /var/jenkins_home/casc.yml

# add plugins
COPY ./plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
