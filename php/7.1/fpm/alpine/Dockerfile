FROM php:7-fpm-alpine
MAINTAINER Voskobovich Vitaly <vitaly@voskobovich.com>

# Copy PHP config
COPY ./config/php.ini /usr/local/etc/php/php.ini

# Install Yii2 requirements
RUN set -xe \
    # Update repository
    && apk update \
    && apk upgrade \

    # Add packages to compile the libraries
    && apk add --no-cache autoconf g++ libtool make \

    # Git
    && apk add --no-cache git \

    # Fixed Intl version
    && apk add --no-cache libintl icu icu-dev \
    && docker-php-ext-install intl \
    && apk del icu-dev \

    && docker-php-ext-install opcache \

    # PDO
    && docker-php-ext-install pdo \

    # PDO MySQL
    && docker-php-ext-install pdo_mysql \

    # PDO PgSQL
    && apk add --no-cache postgresql-dev \
    && docker-php-ext-install pdo_pgsql \

    # GD
    && apk add --no-cache freetype-dev libpng-dev libjpeg-turbo-dev libxml2-dev \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \

    # ImageMagic
    && apk add --no-cache imagemagick-dev pcre-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick \

    # Clear after install GD and ImageMagic
    && apk del --no-cache freetype-dev libjpeg-turbo-dev libxml2-dev \

    # Memcached
    && apk add --no-cache libmemcached-dev zlib-dev cyrus-sasl-dev \
    && docker-php-source extract \
    && git clone --branch php7 https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached/ \
    && docker-php-ext-configure memcached \
    && docker-php-ext-install memcached \
    && docker-php-source delete \
    && apk del --no-cache zlib-dev cyrus-sasl-dev \

    # Clear
    && apk del --no-cache autoconf g++ libtool make \
    && rm -rf /tmp/* /var/cache/apk/*

# Install Composer
ONBUILD ARG GITHUB_OAUTH_TOKEN

ONBUILD RUN set -xe \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g github-oauth.github.com $GITHUB_OAUTH_TOKEN \
    && composer global require "fxp/composer-asset-plugin:^1.2.0"
