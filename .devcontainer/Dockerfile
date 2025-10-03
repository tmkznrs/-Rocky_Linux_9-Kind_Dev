# ベースイメージとしてRocky Linux 9を使用
FROM rockylinux:9

# 開発に必要なパッケージをインストール
RUN dnf install -y \
    git \
#    curl \
    unzip \
    tar \
    sudo \
    && dnf clean all

# Dockerクライアントのインストール
# dnf経由でDockerクライアントをインストールするためのリポジトリを追加
RUN dnf install -y dnf-utils \
    && dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo \
    && dnf install -y docker-ce-cli \
    && dnf clean all
    
# Kindをインストール
ARG KIND_VERSION=v0.22.0
RUN curl -Lo ./kind "https://kind.sigs.k8s.io/dl/${KIND_VERSION}/kind-linux-amd64" \
    && chmod +x ./kind \
    && mv ./kind /usr/local/bin/kind

# kubectlをインストール
ARG KUBECTL_VERSION=v1.29.0
RUN curl -Lo ./kubectl "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/kubectl

# ユーザーの作成と権限設定
RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid 1000 -m -s /bin/bash vscode \
    && echo "vscode ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# ユーザーをvscodeに切り替え
USER vscode

WORKDIR /workspaces