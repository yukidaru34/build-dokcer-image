# ベースイメージを指定
FROM debian:buster-slim

# パッケージの更新
RUN apt-get update && apt-get install -y wget

# Terraformのインストール
RUN wget -qO- https://releases.hashicorp.com/terraform/1.3.7/terraform_1.3.7_linux_amd64.zip \
  && unzip terraform_1.3.7_linux_amd64.zip \
  && mv terraform /usr/local/bin/
# バージョンは適宜変更してください

# kubectlのインストール
RUN curl -LO https://dl.k8s.io/release/v1.26.0/bin/linux/amd64/kubectl \
  && chmod +x kubectl \
  && mv kubectl /usr/local/bin/
# バージョンは適宜変更してください

# AWS CLIのインストール
RUN curl "https://awscli.amazonaws.com/awscliv2.zip" -o "awscliv2.zip" \
  && unzip awscliv2.zip \
  && ./aws/install
# バージョンは適宜変更してください

# 環境変数の設定 (例)
ENV PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

# 作業ディレクトリの指定
WORKDIR /app

# ユーザの作成 (任意)
RUN useradd -m appuser
USER appuser