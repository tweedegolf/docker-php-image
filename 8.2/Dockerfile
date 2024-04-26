ARG DEBIAN_VERSION
FROM ghcr.io/tweedegolf/debian:${DEBIAN_VERSION}

# Install postgresql client
ARG POSTGRESQL_VERSION
ENV POSTGRESQL_VERSION ${POSTGRESQL_VERSION}
RUN curl -s -L https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && echo "deb http://apt.postgresql.org/pub/repos/apt/ bookworm-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        postgresql-client-$POSTGRESQL_VERSION \
    && rm -rf /var/lib/apt/lists/*

# Install the custom PHP repository
ENV PHP_VERSION 8.2
RUN curl -s -L https://packages.sury.org/php/apt.gpg | apt-key add - \
    && echo "deb https://packages.sury.org/php/ bookworm main" > /etc/apt/sources.list.d/sury_php.list \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        php${PHP_VERSION}-bcmath \
        php${PHP_VERSION}-cli \
        php${PHP_VERSION}-common \
        php${PHP_VERSION}-curl \
        php${PHP_VERSION}-dev \
        php${PHP_VERSION}-enchant \
        php${PHP_VERSION}-fpm \
        php${PHP_VERSION}-gd \
        php${PHP_VERSION}-gmp \
        php${PHP_VERSION}-intl \
        php${PHP_VERSION}-mbstring \
        php${PHP_VERSION}-mysql \
        php${PHP_VERSION}-odbc \
        php${PHP_VERSION}-opcache \
        php${PHP_VERSION}-pgsql \
        php${PHP_VERSION}-soap \
        php${PHP_VERSION}-sqlite3 \
        php${PHP_VERSION}-tidy \
        php${PHP_VERSION}-xml \
        php${PHP_VERSION}-xsl \
        php${PHP_VERSION}-zip \
        php${PHP_VERSION}-apcu \
        php${PHP_VERSION}-ast \
        php${PHP_VERSION}-imagick \
        php${PHP_VERSION}-memcache \
        php${PHP_VERSION}-memcached \
        php${PHP_VERSION}-redis \
        php-pear \
    && rm -rf /var/lib/apt/lists/*
COPY php-fpm.conf /etc/php/${PHP_VERSION}/fpm/php-fpm.conf
COPY php.ini /etc/php/${PHP_VERSION}/fpm/php.ini
COPY php.ini /etc/php/${PHP_VERSION}/cli/php.ini

# Install composer
ARG COMPOSER_VERSION
ENV COMPOSER_VERSION ${COMPOSER_VERSION}
RUN curl -L -s -o /usr/local/bin/composer https://getcomposer.org/download/$COMPOSER_VERSION/composer.phar \
    && chmod a+x /usr/local/bin/composer

CMD [ "/usr/sbin/php-fpm8.2" ]
EXPOSE 9000
