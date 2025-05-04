FROM jenkins/jenkins

ENV JENKINS_USER admin
ENV JENKINS_PASS admin

# Пропускаємо початкове налаштування
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

# Копіюємо список плагінів та встановлюємо їх
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN jenkins-plugin-cli --latest
RUN jenkins-plugin-cli -f /usr/share/jenkins/plugins.txt

# Копіюємо конфігурацію job
COPY config.xml /usr/share/jenkins/ref/jobs/hello_world/config.xml

# Встановлення Docker в контейнер
USER root
RUN apt-get update \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common

RUN mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | tee /etc/apt/keyrings/docker.asc > /dev/null \
    && chmod a+r /etc/apt/keyrings/docker.asc

RUN echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" | tee /etc/apt/sources.list.d/docker.list

RUN apt-get update -qq \
    && apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin \
    && apt-get clean

# Додаємо Jenkins у групу Docker
RUN usermod -aG docker jenkins

# Встановлення Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

USER jenkins

