FROM cjsewell/ss-yarn:7.3
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

RUN set -ex \
    && install-php-extensions xdebug && \
        sed -i '1 a xdebug.remote_autostart=true' "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_mode=req'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_handler=dbgp'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_connect_back=1'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_port=9000'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_host=127.0.0.1'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i '1 a xdebug.remote_enable=1'  "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
        sed -i 's/validate_timestamps=0/validate_timestamps=1/' "$PHP_INI_DIR/conf.d/docker-php-ext-opcache.ini"

