FROM cjsewell/ss-yarn:7.2
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

# Install components
RUN apt-get -yq update
RUN apt-get install -yq rsync
RUN composer global require --no-ansi --prefer-dist --optimize-autoloader deployer/deployer:^6.1
RUN yarn global add envsub
