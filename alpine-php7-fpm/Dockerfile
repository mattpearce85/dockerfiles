FROM php:7.2.11-fpm-alpine3.8
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV PHP_APCU_VERSION 5.1.12
ENV PHP_MCRYPT_VERSION 1.0.1
ENV PHP_XDEBUG_VERSION 2.6.0
ENV PHP_YAML_VERSION 2.0.2

RUN docker-php-source extract \
    && set -xe \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
    && apk add \ 
        freetype \
        libpng \
        libjpeg-turbo \
        freetype-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        openldap-dev \
        imap-dev \
        icu-dev \
        unixodbc-dev \
        postgresql-dev \
        libxml2-dev \
        libmcrypt-dev \
        libltdl \
        yaml \
        yaml-dev \
    && docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && { \
        echo '# https://github.com/docker-library/php/issues/103#issuecomment-271413933'; \
        echo 'AC_DEFUN([PHP_ALWAYS_SHARED],[])dnl'; \
        echo; \
        cat /usr/src/php/ext/odbc/config.m4; \
    } > temp.m4 \
    && mv temp.m4 /usr/src/php/ext/odbc/config.m4 \
    && docker-php-ext-configure odbc \
        --with-unixODBC="shared,/usr" \
    && CFLAGS="-I/usr/src/php" docker-php-ext-install \
        bcmath \
        gd \
        imap \
        intl \
        json \
        ldap \
        mysqli \
        odbc \
        opcache \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        soap \
        sockets \
        xmlreader \
        xmlwriter \
        zip \
    && pecl install apcu-${PHP_APCU_VERSION} \
    && pecl install mcrypt-${PHP_MCRYPT_VERSION} \
    && pecl install xdebug-${PHP_XDEBUG_VERSION} \
    && pecl install yaml-${PHP_YAML_VERSION} \
    && docker-php-ext-enable \
        apcu \
        mcrypt \
        xdebug \
        yaml \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /tmp/* /var/cache/apk/*
    
