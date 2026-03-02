FROM jenkins/jenkins:lts

USER root

# 1. Install Docker CLI and dependencies
RUN apt-get update && \
    apt-get install -y docker.io curl ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# 2. Define plugin directory
ENV DOCKER_PLUGINS=/usr/local/lib/docker/cli-plugins
RUN mkdir -p $DOCKER_PLUGINS

# 3. Install Docker Compose v2 (Latest)
RUN curl -SL https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-linux-x86_64 \
    -o $DOCKER_PLUGINS/docker-compose && \
    chmod +x $DOCKER_PLUGINS/docker-compose

# 4. Install Docker Buildx (Fixes the 0.17.0 error)
RUN curl -SL https://github.com/docker/buildx/releases/download/v0.17.1/buildx-v0.17.1.linux-amd64 \
    -o $DOCKER_PLUGINS/docker-buildx && \
    chmod +x $DOCKER_PLUGINS/docker-buildx

# 5. Install kubectl
RUN curl -LO "https://dl.k8s.io/release/v1.30.0/bin/linux/amd64/kubectl" && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl

# 6. Setup Docker Group Permissions
# Force the docker group to match the host GID if needed, but 'groupadd -f' is a good start
RUN groupadd -f docker && usermod -aG docker jenkins

# Verify installations during build
RUN docker compose version && docker buildx version

USER jenkins