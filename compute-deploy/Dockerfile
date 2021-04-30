FROM cjsewell/ss-yarn:7.3
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

ENV SOPS_VERSION 3.7.1
ENV SOPS_BINARY=/usr/bin/sops

RUN set -ex \
    && apt-get update -y \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        gnupg \
        rsync \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -r /var/lib/apt/lists/* \
    && yarn global add envsub \
    && yarn cache clean

RUN set -ex \
    && echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list \
    && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - \
    && apt-get update -y \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        google-cloud-sdk \
        python3-crcmod \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -r /var/lib/apt/lists/*

RUN ARCH= && ARCH="$(dpkg --print-architecture)" \
    && set -ex \
    && curl -fsSLO https://github.com/mozilla/sops/releases/download/v${SOPS_VERSION}/sops_${SOPS_VERSION}_${ARCH}.deb \
    && dpkg -i sops_${SOPS_VERSION}_${ARCH}.deb \
    && rm sops_${SOPS_VERSION}_${ARCH}.deb \
    # smoke test
    && sops --version
