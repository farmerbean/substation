FROM --platform=linux/arm64 mcr.microsoft.com/vscode/devcontainers/go:dev-1.22

# Set environment variables
ENV GOINSECURE="github.com,proxy.golang.org,goproxy.io" \
    GIT_SSL_NO_VERIFY=true \
    GOPROXY=https://goproxy.io,direct

# Disable SSL verification globally for APT and wget
RUN echo 'Acquire::https::Verify-Peer "false";' > /etc/apt/apt.conf.d/99verify-peer
RUN echo 'check-certificate = off' > ~/.wgetrc
RUN git config --global http.sslVerify false

# Update system and install necessary packages
RUN apt-get update -y && apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    && update-ca-certificates 2>/dev/null || true

# Install Docker
RUN curl -kfsSL https://get.docker.com | sh

# Configure Go environment and install go-jsonnet tools
RUN go env -w CGO_ENABLED=0 && \
    go install github.com/google/go-jsonnet/cmd/jsonnet@latest && \
    go install github.com/google/go-jsonnet/cmd/jsonnetfmt@latest && \
    go install github.com/google/go-jsonnet/cmd/jsonnet-lint@latest

# Add HashiCorp's repository and install Terraform
RUN wget --no-check-certificate -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [allow-insecure=yes signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list && \
    apt-get update -y && apt-get install -y terraform

# Install AWS CLI and Python tools
RUN apt-get install -y awscli python3 python3-boto3 black

# Clean up to reduce image size
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*