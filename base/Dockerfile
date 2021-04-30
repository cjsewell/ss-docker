FROM php:7.3-apache-buster
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/root/.composer/vendor/bin:${PATH}"
ENV COMPOSER_ALLOW_SUPERUSER=1

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/install-php-extensions

# Install components
RUN set -ex \
    && apt-get update -y \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        curl \
        git-core \
        gzip \
        libcurl4-openssl-dev \
        libzip-dev \
        libgd-dev \
        libldap2-dev \
        libmcrypt-dev \
        libtidy-dev \
        libxslt-dev \
        libpng-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        openssh-client \
        libmagickwand-dev \
        unzip \
        zip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
    && curl -sS https://silverstripe.github.io/sspak/install | php -- /usr/local/bin \
    && curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer \
    && apt-get remove -y libmagickwand-dev \
    && apt-get install -y libmagickwand-6.q16-6 \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -r /var/lib/apt/lists/*

# Install PHP Extensions
RUN install-php-extensions \
        bcmath \
        mysqli \
        pdo \
        pdo_mysql \
        intl \
        ldap \
        gd \
        soap \
        tidy \
        xsl \
        zip \
        exif \
        gmp \
        opcache \
        grpc

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN set -eux; \
	docker-php-ext-enable opcache; \
	{ \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
	} > "$PHP_INI_DIR/conf.d/opcache-recommended.ini"

# Set error handling
RUN echo 'date.timezone = Pacific/Auckland' > "$PHP_INI_DIR/conf.d/timezone.ini" && \
    { \
        echo 'log_errors = On'; \
        echo 'error_log = /dev/stderr'; \
    } > "$PHP_INI_DIR/conf.d/errors.ini"

# Apache configuration
ENV DOCUMENT_ROOT /var/www/html/public
RUN { \
        echo '<VirtualHost *:80>'; \
        echo '  DocumentRoot ${DOCUMENT_ROOT}'; \
        echo '  LogLevel warn'; \
        echo '  ServerSignature Off'; \
        echo '  <Directory ${DOCUMENT_ROOT}>'; \
        echo '    Options +FollowSymLinks'; \
        echo '    Options -ExecCGI -Includes -Indexes'; \
        echo '    AllowOverride all'; \
        echo; \
        echo '    Require all granted'; \
        echo '  </Directory>'; \
        echo '  <LocationMatch assets/>'; \
        echo '    php_flag engine off'; \
        echo '  </LocationMatch>'; \
        echo; \
        echo '  IncludeOptional sites-available/000-default.local*'; \
        echo '</VirtualHost>'; \
	} > /etc/apache2/sites-available/000-default.conf \
    && echo "ServerName localhost" > /etc/apache2/conf-available/fqdn.conf \
    && a2enmod rewrite expires remoteip headers \
    && a2enconf fqdn \
    && usermod -u 1000 www-data \
    && usermod -G staff www-data

EXPOSE 80
CMD ["apache2-foreground"]
