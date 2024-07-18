FROM --platform=linux/arm64 mcr.microsoft.com/vscode/devcontainers/go:dev-1.22
ENV GOINSECURE="GitHub.com,proxy.golang.org" \
    GIT_SSL_NO_VERIFY=true

RUN echo 'Acquire::https::Verify-Peer "false";' > /etc/apt/apt.conf.d/99verify-peer
RUN echo 'check-certificate = off' > ~/.wgetrc
RUN git config --global http.sslVerify false
RUN apk update \
        && apk upgrade \
        && apk add --no-cache \
        ca-certificates \
        && update-ca-certificates 2>/dev/null || true
RUN apt-get update -y && \
    # Docker
    curl -kfsSL https://get.docker.com | sh && \
    # Terraform
    wget --no-check-certificate -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [allow-insecure=yes signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list && \
    apt update -y && apt install -y terraform && \
    # AWS CLI
    apt-get install -y awscli && \
    # python
    apt-get install -y python3 python3-boto3 black \
    # go-jsonnet
    go install github.com/google/go-jsonnet/cmd/jsonnet@latest && \
    go install github.com/google/go-jsonnet/cmd/jsonnetfmt@latest && \
    go install github.com/google/go-jsonnet/cmd/jsonnet-lint@latest
