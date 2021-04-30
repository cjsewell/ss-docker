FROM cjsewell/ss-yarn:7.4
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
    && yarn cache clean \

RUN ARCH= && ARCH="$(dpkg --print-architecture)" \
    && set -ex \
    && curl -fsSLO https://github.com/mozilla/sops/releases/download/v${SOPS_VERSION}/sops_${SOPS_VERSION}_${ARCH}.deb \
    && dpkg -i sops_${SOPS_VERSION}_${ARCH}.deb \
    && rm sops_${SOPS_VERSION}_${ARCH}.deb \
    # smoke test
    && sops --version
