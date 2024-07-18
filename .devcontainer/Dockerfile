FROM --platform=linux/arm64 mcr.microsoft.com/vscode/devcontainers/go:dev-1.22
RUN apt-get -o "Acquire::https::Verify-Peer=false" update -y && \
    # Docker
    echo "check-certificate = off" | sudo tee --append ~/.wgetrc && \
    curl -kfsSL https://get.docker.com | sh && \
    # https://developer.hashicorp.com/terraform/downloads
    wget --no-check-certificate -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [allow-insecure=yes signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list && \
    apt -o "Acquire::https::Verify-Peer=false" update -y && apt -o "Acquire::https::Verify-Peer=false" install -y terraform && \
    # go-jsonnet
    export GOINSECURE=github.com,proxy.golang.org && go install github.com/google/go-jsonnet/cmd/jsonnet@latest && \
    export GOINSECURE=github.com,proxy.golang.org && go install github.com/google/go-jsonnet/cmd/jsonnetfmt@latest && \
    export GOINSECURE=github.com,proxy.golang.org && go install github.com/google/go-jsonnet/cmd/jsonnet-lint@latest && \
    # AWS CLI
    apt-get -o "Acquire::https::Verify-Peer=false" install -y awscli && \
    # python
    apt-get -o "Acquire::https::Verify-Peer=false" install -y python3 python3-boto3 black
