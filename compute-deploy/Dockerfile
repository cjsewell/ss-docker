FROM cjsewell/ss-deployer:7.2
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

ENV SOPS_VERSION=3.3.0
ENV SOPS_BINARY=/usr/bin/sops
ENV CLOUD_SDK_VERSION=240.0.0

RUN curl -sSL -o "${SOPS_BINARY}" "https://github.com/mozilla/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.linux"
RUN chmod +x "${SOPS_BINARY}"
RUN curl -sSL https://deb.nodesource.com/setup_10.x | bash -
RUN curl -sSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN curl -sSL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://packages.cloud.google.com/apt cloud-sdk main" > /etc/apt/sources.list.d/google-cloud-sdk.list
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list
RUN apt-get -yq update && apt-get install -yq --no-install-recommends \
        google-cloud-sdk \
        yarn && \
        apt-get autoremove -y && \
        apt-get clean -y && \
        rm -r /var/lib/apt/lists/*
