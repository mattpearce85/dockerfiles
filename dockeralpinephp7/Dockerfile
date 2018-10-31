FROM alpine:3.8
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV PHALCON_VERSION="3.2.1"

RUN apk --no-cache add \
        tini \
        php7 \
        php7-apcu \
        php7-bcmath \
        php7-ctype \
        php7-curl \
        php7-dom \
        php7-fileinfo \
        php7-fpm \
        php7-ftp \
        php7-gd \
        php7-iconv \
        php7-imap \
        php7-intl \
        php7-json \
        php7-ldap \
        php7-mbstring \
        php7-memcached \
        php7-mcrypt \
        php7-mysqli \
        php7-mysqlnd \
        php7-odbc \
        php7-opcache \
        php7-openssl \
        php7-pcntl \
        php7-pdo \
        php7-pdo_mysql \
        php7-pdo_pgsql \
        php7-pdo_sqlite \
        php7-pear \
        php7-phar \
        php7-posix \
        php7-session \
        php7-simplexml \
        php7-soap \
        php7-sockets \
        php7-sodium \
        php7-sqlite3 \
        php7-tokenizer \
        php7-xdebug \
        php7-xmlreader \
        php7-xmlwriter \
        php7-yaml \
        php7-zip \
        php7-zlib

EXPOSE 9000
CMD ["php-fpm7"]

