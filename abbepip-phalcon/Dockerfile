FROM vastblueshift/alpine-php7-fpm:latest
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV PHALCON_VERSION 3.4.1
ENV PHALCON_DEVTOOLS_VERSION 3.4.0

RUN docker-php-source extract \
    && set -xe \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
    # Compile Phalcon
    && curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar -xzf v${PHALCON_VERSION}.tar.gz && cd cphalcon-${PHALCON_VERSION}/build && sh install \
    && echo "extension=phalcon.so" > $PHP_INI_DIR/conf.d/phalcon.ini \
    && cd ../.. && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} \
    # Install Phalcon Devtools, see https://github.com/phalcon/phalcon-devtools/
    && curl -LO https://github.com/phalcon/phalcon-devtools/archive/v${PHALCON_DEVTOOLS_VERSION}.tar.gz \
    && tar -xzf v${PHALCON_DEVTOOLS_VERSION}.tar.gz \
    && mv phalcon-devtools-${PHALCON_DEVTOOLS_VERSION} /usr/local/phalcon-devtools \
    && ln -s /usr/local/phalcon-devtools/phalcon.php /usr/local/bin/phalcon \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /tmp/* /var/cache/apk/*

