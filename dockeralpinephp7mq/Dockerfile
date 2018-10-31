FROM alpine:3.8
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

RUN apk --no-cache add \
        tini \
        php7 \
        php7-apcu \
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

# install and remove building packages
ENV PHPIZE_DEPS autoconf file g++ gcc libc-dev make pkgconf re2c php7-dev php7-pear \
    libevent-dev openssl-dev curl rpm

ENV PHP_INI_DIR /etc/php7

RUN set -xe \
    && apk add --no-cache --repository "http://dl-cdn.alpinelinux.org/alpine/edge/testing" \
    --virtual .phpize-deps \
    $PHPIZE_DEPS \
    && sed -i 's/^exec $PHP -C -n/exec $PHP -C/g' $(which pecl) \
    && pecl channel-update pecl.php.net \
    && curl -fsSL 'https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/messaging/mqadv/mqadv_dev904_linux_x86-64.tar.gz' -o mqc.tar.gz \
    && mkdir -p mqc \
    && tar -xf mqc.tar.gz -C mqc --strip-components=1 \
    && rm mqc.tar.gz \
    && ( \
        cd mqc \
        && sed -i 's/^UNAME_FLAG=-i/UNAME_FLAG=-m/' ./mqlicense.sh \
        && ./mqlicense.sh -accept \
        && rpm -i MQSeriesRuntime-9.0.4-0.x86_64.rpm \
        && rpm -i MQSeriesClient-9.0.4-0.x86_64.rpm \
        && rpm -i MQSeriesSDK-9.0.4-0.x86_64.rpm \
        && rpm -i MQSeriesSamples-9.0.4-0.x86_64.rpm \
    ) \
    && rm -rf mqc \
    && curl -fsSL 'https://github.com/rstmpw/pecl-networking-mqseries/archive/php7-mqmh.tar.gz' -o mqseries.tar.gz \
    && mkdir -p mqseries \
    && tar -xf mqseries.tar.gz -C mqseries --strip-components=1 \
    && rm mqseries.tar.gz \
    && ( \
        cd mqseries && ls -lia && nhjklasbnkdlf \
        && phpize \
        && ./configure --with-libdir=lib64 \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -rf /opt/mqm/ \
    && echo "extension=mqseries.so" > $PHP_INI_DIR/conf.d/01_mqseries.ini \
    && pecl install oci8-2.1.8 \
    && echo "extension=oci8.so" > $PHP_INI_DIR/conf.d/01_oci8.ini \
    && pecl install PDO_OCI-1.0 \
    && echo "extension=pdo_oci.so" > $PHP_INI_DIR/conf.d/01_pdo_oci.ini \
    # Compile Phalcon
    && curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz && cd cphalcon-${PHALCON_VERSION}/build && sh install \
    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini \
    && cd ../.. && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} \
    # Install Phalcon Devtools, see https://github.com/phalcon/phalcon-devtools/
    && curl -LO https://github.com/phalcon/phalcon-devtools/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && mv phalcon-devtools-${PHALCON_VERSION} /usr/local/phalcon-devtools \
    && ln -s /usr/local/phalcon-devtools/phalcon.php /usr/local/bin/phalcon \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .phpize-deps

RUN set -x \
    && addgroup -g 82 -S www-data \
    && adduser -u 82 -D -S -G www-data www-data

ENTRYPOINT ["/sbin/tini", "--"]

CMD ["/usr/sbin/php-fpm7", "-R", "--nodaemonize"]

EXPOSE 9000

WORKDIR /app

