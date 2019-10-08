FROM php:7.2-apache
MAINTAINER Corey Sewell "<corey@sewell.net.nz>"
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/root/.composer/vendor/bin:${PATH}"
ENV COMPOSER_ALLOW_SUPERUSER=1

# Install components
RUN apt-get update -y && apt-get install -y --no-install-recommends \
        curl \
        git-core \
        gzip \
        libcurl4-openssl-dev \
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
        libjpeg62-turbo-dev && \
    curl -sS https://silverstripe.github.io/sspak/install | php -- /usr/local/bin && \
    curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && \
    pecl install imagick-3.4.4 && \
    pecl install mcrypt-1.0.2 && \
    apt-get remove -y libmagickwand-dev && \
    apt-get install -y libmagickwand-6.q16-6 && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    rm -r /var/lib/apt/lists/*

# Install PHP Extensions
RUN docker-php-ext-configure intl && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-enable imagick mcrypt && \
    docker-php-ext-install opcache && \
    docker-php-ext-install -j$(nproc) \
        intl \
        ldap \
        mysqli \
        pdo \
        pdo_mysql \
        soap \
        tidy \
        xsl \
        zip

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd -j$(nproc) gd

# Apache configuration
RUN { \
        echo "<VirtualHost *:80>"; \
        echo "  DocumentRoot /var/www/html"; \
        echo "  LogLevel warn"; \
        echo "  ErrorLog /var/log/apache2/error.log"; \
        echo "  CustomLog /var/log/apache2/access.log combined"; \
        echo "  ServerSignature Off"; \
        echo "  <Directory /var/www/html>"; \
        echo "    Options +FollowSymLinks"; \
        echo "    Options -ExecCGI -Includes -Indexes"; \
        echo "    AllowOverride all"; \
        echo; \
        echo "    Require all granted"; \
        echo "  </Directory>"; \
        echo "  <LocationMatch assets/>"; \
        echo "    php_flag engine off"; \
        echo "  </LocationMatch>"; \
        echo; \
        echo "  IncludeOptional sites-available/000-default.local*"; \
        echo "</VirtualHost>"; \
    } | tee /etc/apache2/sites-available/000-default.conf

RUN echo "ServerName localhost" > /etc/apache2/conf-available/fqdn.conf

RUN a2enconf fqdn && \
    a2enmod rewrite expires remoteip cgid && \
    usermod -u 1000 www-data && \
    usermod -G staff www-data

# PHP configuration
RUN echo "date.timezone = Pacific/Auckland" > /usr/local/etc/php/conf.d/timezone.ini
RUN { \
        echo "log_errors = On"; \
        echo "error_log = /dev/stderr"; \
    } | tee /usr/local/etc/php/conf.d/error-logging.ini

RUN { \
        echo "opcache.revalidate_freq=0"; \
        echo "opcache.validate_timestamps=0"; \
        echo "opcache.max_accelerated_files=7963"; \
        echo "opcache.memory_consumption=192"; \
        echo "opcache.interned_strings_buffer=16"; \
        echo "opcache.fast_shutdown=1"; \
    } | tee /usr/local/etc/php/conf.d/opcache.ini

EXPOSE 80
CMD ["apache2-foreground"]
