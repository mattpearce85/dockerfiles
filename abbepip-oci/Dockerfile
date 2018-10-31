FROM vastblueshift/abbepip-php7:phalcon
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

ENV LD_LIBRARY_PATH /opt/oracle/instantclient_12_1
ENV TNS_ADMIN /opt/oracle/instantclient_12_1
ENV ORACLE_BASE /opt/oracle/instantclient_12_1
ENV ORACLE_HOME /opt/oracle/instantclient_12_1

RUN docker-php-source extract \
    && set -xe \
    && apk add libnsl libaio \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        unzip \
        wget

# Install Oracle Instantclient
RUN mkdir /opt && mkdir /opt/oracle \
    && cd /opt/oracle \
    && wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.1.0.2.0.zip \
    && wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-sdk-linux-12.1.0.2.0.zip \
    && unzip /opt/oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && unzip /opt/oracle/instantclient-sdk-linux-12.1.0.2.0.zip -d /opt/oracle \
    && ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
    && ln -s /opt/oracle/instantclient_12_1/libclntshcore.so.12.1 /opt/oracle/instantclient_12_1/libclntshcore.so \
    && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
    && rm -rf /opt/oracle/*.zip

# Install PHP extensions
RUN echo 'instantclient,/opt/oracle/instantclient_12_1' | pecl install oci8 \
    && docker-php-ext-configure oci8 \
        --with-oci8=instantclient,/opt/oracle/instantclient_12_1 \
    && docker-php-ext-configure pdo_oci \
        --with-pdo_oci=instantclient,/opt/oracle/instantclient_12_1 \
    && docker-php-ext-install \
        oci8 \
        pdo_oci \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /tmp/* /var/cache/apk/*

