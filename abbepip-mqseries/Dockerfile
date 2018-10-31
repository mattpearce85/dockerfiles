FROM vastblueshift/abbepip-php7:phalcon
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

# The URL to download the MQ installer from in tar.gz format
ARG MQ_URL=http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/messaging/mqadv/mqadv_dev90_linux_x86-64.tar.gz

# The MQ packages to install
ARG MQ_PACKAGES="MQSeriesRuntime-*.rpm MQSeriesServer-*.rpm MQSeriesMsg*.rpm MQSeriesJava*.rpm MQSeriesJRE*.rpm MQSeriesGSKit*.rpm"

RUN docker-php-source extract \
    && set -xe \
    # Install additional packages required by MQ, this install process and the runtime scripts
    && apk add --no-cache --virtual .build-deps \
        autoconf \
        dpkg-dev dpkg \
        file \
        g++ \
        gcc \
        libc-dev \
        make \
        pkgconf \
        re2c \
        bash \
        bc \
        coreutils \
        curl \
        findutils \
        gawk \
        grep \
        procps \
        rpm \
        sed \
        tar \
        util-linux \
    # Download and extract the MQ installation files
    && mkdir -p /tmp/mq \
    && cd /tmp/mq \
    && curl -LO $MQ_URL \
    && tar -zxvf ./*.tar.gz \
    # Recommended: Create the mqm user ID with a fixed UID and group, so that the file permissions work between different images
    && addgroup -S -g 1000 mqm \
    && adduser -u 1000 -D -S -G mqm -h /var/mqm mqm \
    && cd /tmp/mq/MQServer \
    # Accept the MQ license
    && ./mqlicense.sh -text_only -accept \
    # Install MQ using the RPM packages
    && rpm -ivh $MQ_PACKAGES \
    # Recommended: Set the default MQ installation (makes the MQ commands available on the PATH)
    && /opt/mqm/bin/setmqinst -p /opt/mqm -i \
    # Clean up all the downloaded files
    && curl -fsSL 'https://github.com/rstmpw/pecl-networking-mqseries/archive/php7-mqmh.tar.gz' -o mqseries.tar.gz \
    && mkdir -p mqseries \
    && tar -xf mqseries.tar.gz -C mqseries --strip-components=1 \
    && rm mqseries.tar.gz \
    && ( \
        cd mqseries \
        && phpize \
        && ./configure --with-libdir=lib64 \
        && make -j$(nproc) \
        && make install \
    ) \
    && echo "extension=mqseries.so" > $PHP_INI_DIR/conf.d/01_mqseries.ini \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /tmp/mq \
    && rm -rf /tmp/* /var/cache/apk/*

RUN chmod +x /usr/local/bin/*.sh

# Always use port 1414 (the Docker administrator can re-map ports at runtime)
EXPOSE 1414

# Always put the MQ data directory in a Docker volume
VOLUME /var/mqm

