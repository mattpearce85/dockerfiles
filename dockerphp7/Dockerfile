FROM php:7.2-fpm
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV PHP_APCU_VERSION 5.1.12
ENV PHP_MCRYPT_VERSION 1.0.1
ENV PHP_XDEBUG_VERSION 2.6.0

RUN apt-get update && apt-get upgrade -y && apt-get autoremove -y && apt-get install -y \
    libmcrypt-dev \
    libicu-dev \
    libxml2-dev \
    autoconf \
    cmake \
    file \
    g++ \
    gcc \
    libc-dev \
    make \
    git \
    pkgconf \
    re2c \
    libpng-dev \
    libjpeg-dev \
    libpq-dev

RUN rm -rf /var/lib/apt/lists/*

RUN docker-php-source extract \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-configure mysqli --with-mysqli \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure pdo_pgsql --with-pdo-pgsql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install -j$(nproc) \
        gd \
        bcmath \
        intl \
        pcntl \
        mysqli \
        pdo_mysql \
        pdo_pgsql \
        mbstring \
        soap \
        iconv \
    && pecl install apcu-${PHP_APCU_VERSION} \
    && pecl install mcrypt-${PHP_MCRYPT_VERSION} \
    && pecl install xdebug-${PHP_XDEBUG_VERSION} \
    && docker-php-ext-enable apcu mcrypt xdebug \
    && docker-php-source delete

RUN echo "xdebug.remote_enable=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=off\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=9000\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=0\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && rm -rf /tmp/*

