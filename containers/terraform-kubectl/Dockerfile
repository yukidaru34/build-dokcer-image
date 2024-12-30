FROM ubuntu:latest AS builder

# パッケージのインストール
RUN apt-get update && apt-get install -y wget curl unzip && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Terraformのインストール
ENV TERRAFORM_VERSION=1.3.7

# Terraformのインストール
RUN wget -qO- https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
  && unzip -q terraform_${TERRAFORM_VERSION}_linux_amd64.zip || exit 1 \
  && mkdir -p /usr/local/bin \
  && mv terraform /usr/local/bin/
# kubectlのインストール
ENV KUBECTL_VERSION v1.26.0
RUN curl -LO https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
  && chmod +x kubectl \
  && mv kubectl /usr/local/bin/

# AWS CLIのインストール
RUN curl "https://awscli.amazonaws.com/awscliv2.zip" -o "awscliv2.zip" \
  && unzip awscliv2.zip \
  && ./aws/install

FROM builder AS final

WORKDIR /app

RUN useradd -m appuser
USER appuser

ENV PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

COPY --from=builder /usr/local/bin/terraform /usr/local/bin/
COPY --from=builder /usr/local/bin/kubectl /usr/local/bin/
COPY --from=builder /usr/local/aws/cli /usr/local/aws/cli