FROM cjsewell/ss-base:7.2
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1
# Install components
RUN curl -sSL https://deb.nodesource.com/setup_lts.x | bash - \
    && curl -sSL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt-get update -y \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        nodejs \
        yarn \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -r /var/lib/apt/lists/*
