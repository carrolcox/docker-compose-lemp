FROM php:7-fpm-alpine

ENV PHP_EXTENSIONS \
    bcmath \
    # bz2 \
    # calendar \
    # dba \
    # enchant \
    # exif \
    gd \
    # gettext \
    # gmp \
    # imap \
    # interbase \
    # intl \
    # ldap \
    mysqli \
    # oci8 \
    # odbc \
    # opcache \
    # pcntl \
    # pdo_dblib \
    # pdo_firebird \
    pdo_mysql \
    # pdo_oci \
    # pdo_odbc \
    # pdo_pgsql \
    # pgsql \
    # pspell \
    # recode \
    # shmop \
    # snmp \
    # soap \
    # sockets \
    # sysvmsg \
    # sysvsem \
    # sysvshm \
    # tidy \
    # wddx \
    # xmlrpc \
    # xsl \
    # zend_test \
    zip

ENV PHP_EXTENSIONS_REQUIREMENTS \
        libpng-dev \
        libzip-dev \
        zip

## Install and enable extensions
RUN set -euxo pipefail \
    && apk add --no-cache ${PHP_EXTENSIONS_REQUIREMENTS} << "/dev/null" \
    && apk add --no-cache --virtual .build-deps ${PHPIZE_DEPS} << "/dev/null" \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install ${PHP_EXTENSIONS} << "/dev/null" \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && apk del --no-network .build-deps ${PHPIZE_DEPS} << "/dev/null" \
    && rm -vfr /tmp/pear ~/.pearrc \
    && echo 'Done.'

