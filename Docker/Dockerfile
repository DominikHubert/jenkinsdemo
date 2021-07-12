FROM jenkins/jenkinsfile-runner
RUN apt update
RUN apt-get install -y npm && npm install -g csslint
RUN wget https://download.docker.com/linux/static/stable/x86_64/docker-20.10.7.tgz
RUN tar xzvf docker-20.10.7.tgz && cp docker/* /usr/bin/
RUN dockerd &

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN cd /app/jenkins && jar -cvf jenkins.war *
RUN java -jar /app/bin/jenkins-plugin-manager.jar --war /app/jenkins/jenkins.war --plugin-file /usr/share/jenkins/ref/plugins.txt && rm /app/jenkins/jenkins.war