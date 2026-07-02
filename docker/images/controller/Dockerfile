FROM jenkins/jenkins:lts-jdk21
USER root
RUN apt-get update && apt-get install -y lsb-release vim wget
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli 
COPY --chown=jenkins:jenkins plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN mkdir -p /usr/share/jenkins/ref/init.groovy.d/
# Write the Groovy logic directly into the reference folder
RUN echo 'import java.lang.System' > /usr/share/jenkins/ref/init.groovy.d/disable-stageview-flag.groovy && \
    echo 'System.setProperty("org.jenkinsci.pipeline.stageview.disabledOnMainJobPage", "false")' >> /usr/share/jenkins/ref/init.groovy.d/disable-stageview-flag.groovy && \
    echo 'println "::: Forced Stage View Visibility via Dockerfile Init Script :::"' >> /usr/share/jenkins/ref/init.groovy.d/disable-stageview-flag.groovy
USER jenkins
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt
