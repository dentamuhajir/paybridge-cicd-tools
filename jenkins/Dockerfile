FROM jenkins/jenkins:lts

USER root

# Install Docker CLI
RUN apt-get update && \
    apt-get install -y docker.io curl && \
    rm -rf /var/lib/apt/lists/*

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/v1.34.1/bin/linux/amd64/kubectl" && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl

# Install kind
RUN curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-amd64 && \
    install -o root -g root -m 0755 kind /usr/local/bin/kind && \
    rm kind

# FIX: Add jenkins user to docker group
RUN groupadd docker || true && usermod -aG docker jenkins

USER jenkins