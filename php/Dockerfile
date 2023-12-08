ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm-alpine

ARG TZ
ARG REPOSITORIES

#替换仓库地址
#RUN sed -i "s/dl-cdn.alpinelinux.org/${REPOSITORIES}/g" /etc/apk/repositories

#安装依赖库
RUN apk --update add \
        autoconf \
        build-base \
        linux-headers \
        libaio-dev \
        zlib-dev \
        curl-dev \
        gettext-dev \
        freetype-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libtool \
        libbz2 \
        bzip2-dev \
        libstdc++ \
        libxslt-dev \
        openldap-dev \
        imagemagick-dev \
        libzip-dev \
        make \
        libmemcached-dev

RUN docker-php-ext-install \
        bcmath \
        pdo_mysql \
        mysqli \
        sockets \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd


ARG INSTALL_PCNTL=false
RUN if [ ${INSTALL_PCNTL} = true ]; then \
    docker-php-ext-install pcntl \
;fi

ARG INSTALL_OPCACHE=false
RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

ARG INSTALL_EXIF=false
RUN if [ ${INSTALL_EXIF} = true ]; then \
    docker-php-ext-install exif \
;fi

ARG INSTALL_BCMATH=false
RUN if [ ${INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
;fi

ARG INSTALL_CALENDAR=false
RUN if [ ${INSTALL_CALENDAR} = true ]; then \
    docker-php-ext-install calendar \
;fi

ARG INSTALL_GETTEXT=false
RUN if [ ${INSTALL_GETTEXT} = true ]; then \
    docker-php-ext-install gettext \
;fi

ARG INSTALL_SHMOP=false
RUN if [ ${INSTALL_SHMOP} = true ]; then \
    docker-php-ext-install shmop \
;fi

ARG INSTALL_SYSVMSG=false
RUN if [ ${INSTALL_SYSVMSG} = true ]; then \
    docker-php-ext-install sysvmsg \
;fi

ARG INSTALL_SYSVSEM=false
RUN if [ ${INSTALL_SYSVSEM} = true ]; then \
    docker-php-ext-install sysvsem \
;fi

ARG INSTALL_SYSVSHM=false
RUN if [ ${INSTALL_SYSVSHM} = true ]; then \
    docker-php-ext-install sysvshm \
;fi

ARG INSTALL_PDO_ODBC=false
RUN if [ ${INSTALL_PDO_ODBC} = true ]; then \
    apk add --no-cache unixodbc-dev && \
    docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr && \
    docker-php-ext-install pdo_odbc \
;fi

ARG INSTALL_PDO_PGSQL=false
RUN if [ ${INSTALL_PDO_PGSQL} = true ]; then \
    apk add --no-cache postgresql-dev && \
    docker-php-ext-install pdo_pgsql \
;fi

ARG INSTALL_PGSQL=false
RUN if [ ${INSTALL_PGSQL} = true ]; then \
    apk add --no-cache postgresql-dev && \
    docker-php-ext-install pgsql \
;fi

ARG INSTALL_DBA=false
RUN if [ ${INSTALL_DBA} = true ]; then \
    docker-php-ext-install dba \
;fi

ARG INSTALL_INTL=false
RUN if [ ${INSTALL_INTL} = true ]; then \
    apk add --no-cache icu-dev && \
    docker-php-ext-install intl \
;fi

ARG INSTALL_BZ2=false
RUN if [ ${INSTALL_BZ2} = true ]; then \
    apk add --no-cache bzip2-dev && \
    docker-php-ext-install bz2 \
;fi

ARG INSTALL_SOAP=false
RUN if [ ${INSTALL_SOAP} = true ]; then \
    apk add --no-cache libxml2-dev && \
    docker-php-ext-install soap \
;fi

ARG INSTALL_XSL=false
RUN if [ ${INSTALL_XSL} = true ]; then \
    apk add --no-cache libxml2-dev libxslt-dev && \
    docker-php-ext-install xsl \
;fi

#ARG INSTALL_XMLRPC=false
#RUN if [ ${INSTALL_XMLRPC} = true ]; then \
#    apk add --no-cache libxml2-dev libxslt-dev && \
#    docker-php-ext-install xmlrpc \
#;fi

ARG INSTALL_SNMP=false
RUN if [ ${INSTALL_SNMP} = true ]; then \
    apk add --no-cache net-snmp-dev && \
    docker-php-ext-install snmp \
;fi

ARG INSTALL_PSPELL=false
RUN if [ ${INSTALL_PSPELL} = true ]; then \
    apk add --no-cache aspell-dev aspell-en && \
    docker-php-ext-install pspell \
;fi

ARG INSTALL_TIDY=false
RUN if [ ${INSTALL_TIDY} = true ]; then \
    apk add --no-cache tidyhtml-dev && \
    ln -s /usr/include/tidybuffio.h /usr/include/buffio.h && \
    docker-php-ext-install tidy \
;fi

ARG INSTALL_GMP=false
RUN if [ ${INSTALL_GMP} = true ]; then \
    apk add --no-cache gmp-dev && \
    docker-php-ext-install gmp \
;fi

ARG INSTALL_IMAP=false
RUN if [ ${INSTALL_IMAP} = true ]; then \
    apk add --no-cache imap-dev && \
    docker-php-ext-configure imap --with-imap --with-imap-ssl && \
    docker-php-ext-install imap \
;fi

ARG INSTALL_LDAP=false
RUN if [ ${INSTALL_LDAP} = true ]; then \
    apk add --no-cache ldb-dev openldap-dev && \
    docker-php-ext-install ldap \
;fi

#安装zip
ARG INSTALL_ZIP=false
RUN if [ ${INSTALL_ZIP} = true ]; then \
    if [ ${PHP_VERSION} = "7.3" ] || [ ${PHP_VERSION} = "7.4" ]; then \
        docker-php-ext-configure zip \
    else \
        docker-php-ext-configure zip --with-libzip \
    ;fi \
    && docker-php-ext-install zip \
;fi

#安装event
# https://github.com/docker-library/php/issues/857
ARG INSTALL_EVENT=false
ARG INSTALL_EVENT_VERSION
RUN if [ ${INSTALL_EVENT} = true ]; then \
    pecl install event-${INSTALL_EVENT_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable --ini-name zz-event.ini event \
;fi

#安装redis
ARG INSTALL_REDIS=false
ARG INSTALL_REDIS_VERSION
RUN if [ ${INSTALL_REDIS} = true ]; then \
    pecl install redis-${INSTALL_REDIS_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
;fi

#安装mongodb
ARG INSTALL_MONGODB=false
ARG INSTALL_MONGODB_VERSION
RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install mongodb-${INSTALL_MONGODB_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable mongodb \
;fi

#安装memcached
ARG INSTALL_MEMCACHED=false
ARG INSTALL_MEMCACHED_VERSION
RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    pecl install memcached-${INSTALL_MEMCACHED_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable memcached \
;fi

#安装swoole
ARG INSTALL_SWOOLE=false
ARG INSTALL_SWOOLE_VERSION
RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    pecl install swoole-${INSTALL_SWOOLE_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable --ini-name zz-swoole.ini swoole \
;fi

#安装composer
ARG INSTALL_COMPOSER=false
RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    curl -sS http://getcomposer.org/installer | php \
    && mv ./composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer \
;fi

#清除安装文件
RUN docker-php-source delete && rm -rf /tmp/pear && rm -rf /var/cache/apk/*

VOLUME /wwwroot
WORKDIR /wwwroot

EXPOSE 9000

CMD ["php-fpm"]