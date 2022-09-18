FROM php:fpm-alpine

LABEL maintainer="Natan Augusto <natanaugusto@gmail.com>"

ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apk update --no-cache

RUN apk add --no-cache --update $PHPIZE_DEPS \
    libjpeg-turbo-dev \
    libmemcached-dev \
    postgresql-dev \
    openldap-dev \
    libxml2-dev \
    libpng-dev \
    libzip-dev \
    zlib-dev \
    imap-dev \
    icu-dev \
    shadow \
    bash \
    g++

RUN docker-php-ext-configure pdo_mysql && docker-php-ext-install pdo_mysql
RUN docker-php-ext-configure pdo_pgsql && docker-php-ext-install pdo_pgsql
RUN docker-php-ext-configure bcmath && docker-php-ext-install bcmath
RUN docker-php-ext-configure intl && docker-php-ext-install intl
RUN docker-php-ext-configure soap && docker-php-ext-install soap
RUN docker-php-ext-configure ldap && docker-php-ext-install ldap
RUN docker-php-ext-configure imap && docker-php-ext-install imap
RUN docker-php-ext-configure zip && docker-php-ext-install zip
RUN docker-php-ext-configure gd && docker-php-ext-install gd
RUN pecl install -f memcached && docker-php-ext-enable memcached
RUN pecl install -f igbinary && docker-php-ext-enable igbinary
RUN pecl install -f msgpack && docker-php-ext-enable msgpack
RUN pecl install -f swoole && docker-php-ext-enable swoole
RUN pecl install -f redis && docker-php-ext-enable redis
RUN pecl install -f pcov && docker-php-ext-enable pcov

ARG INSTALL_COMPOSER=false
RUN if [ $INSTALL_COMPOSER = true ]; then \
    php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
;fi

ARG INSTALL_XDEBUG=false
RUN if [ $INSTALL_XDEBUG = true ]; then \
    pecl install -f xdebug && docker-php-ext-enable xdebug \
;fi

ARG INSTALL_NODE=false
RUN if [ $INSTALL_NODE = true ]; then \
    apk add --no-cache --update nodejs npm \
;fi

ARG WWWGROUP=1000
ARG WWWUSER=1000
RUN groupmod -g $WWWGROUP www-data \
    && usermod -u $WWWUSER www-data \
    && chown www-data:www-data -R /var/www

CMD ["php-fpm"]

EXPOSE 9000
