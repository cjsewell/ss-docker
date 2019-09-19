FROM cjsewell/ss-yarn:7.2
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

# Install components
RUN curl -sS https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 --output /usr/local/bin/cloud_sql_proxy
RUN chmod +x /usr/local/bin/cloud_sql_proxy

RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" > /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl -sS https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
RUN apt-get -yq update
RUN apt-get install -yq google-cloud-sdk && \
        apt-get autoremove -y && \
        apt-get clean -y && \
        rm -r /var/lib/apt/lists/*
