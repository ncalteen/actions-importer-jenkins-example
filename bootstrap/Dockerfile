FROM jenkins/jenkins:2.332.4

ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
ENV CASC_JENKINS_CONFIG /var/jenkins_home/casc.yaml

# add plugins
COPY ./bootstrap/plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# add security perms
COPY ./bootstrap/casc.yaml /var/jenkins_home/casc.yaml

# seed with jobs
COPY ./bootstrap/pipelines /usr/share/jenkins/ref/jobs
