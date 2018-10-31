FROM vastblueshift/ubuntu-16.04-webserver:1.2.0
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV PHP_MCRYPT_VERSION 1.0.1

ENV PHPIZE_DEPS \
    autoconf \
    dpkg-dev \
    file \
    g++ \
    gcc \
    libc-dev \
    make \
    pkg-config \
    re2c

ENV PHP_INI_DIR /etc/php/7.2/fpm

RUN apt-get update && apt-get install -y \
    $PHPIZE_DEPS \
    --no-install-recommends

RUN apt-get install -y \
    php7.2-dev \
    php-pear \
    libmcrypt-dev \
    libreadline-dev

RUN update-alternatives --set php /usr/bin/php7.2 \
    && update-alternatives --set phar /usr/bin/phar7.2 \
    && update-alternatives --set phar.phar /usr/bin/phar.phar7.2 \
    && update-alternatives --set phpize /usr/bin/phpize7.2 \
    && update-alternatives --set php-config /usr/bin/php-config7.2

RUN pecl install mcrypt-${PHP_MCRYPT_VERSION} \
    && echo "extension=mcrypt.so" > $PHP_INI_DIR/conf.d/01_mcrypt.ini

