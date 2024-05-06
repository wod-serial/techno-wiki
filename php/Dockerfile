FROM php:8.1-fpm-alpine

 # сертификаты в старом имадже протухли, так что воруем их из более нового

#mediawiki нужен composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN pear update-channels \
  && pear install Mail Mail_Mime Net_SMTP

RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev libwebp-dev \
    && docker-php-ext-configure gd --enable-gd --with-webp --with-jpeg --with-freetype \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN apk add --no-cache --virtual .build-deps gettext-dev \
    && docker-php-ext-install -j$(nproc) gettext \
    && apk del -f .build-deps

RUN apk add \
        libzip-dev \
        zip \
        zlib-dev \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS imagemagick-dev libtool libgomp \
    && docker-php-ext-install calendar && docker-php-ext-enable calendar \
    && apk add --no-cache imagemagick mysql-client \
    && pecl install imagick  \
    && docker-php-ext-enable imagick \
    && rm -rf /tmp/pear \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysqli \
    && apk del -f .build-deps

RUN apk add --no-cache libmemcached-libs zlib \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS zlib-dev libmemcached-dev \
    && pecl install memcached-3.2.0 \
    && docker-php-ext-enable memcached \
    && apk del -f .build-deps

RUN set -ex \
    && apk --no-cache add icu icu-dev icu-libs \
    && docker-php-ext-install -j$(nproc) intl \
    && apk del icu-dev

RUN apk add libgomp libintl # for imagick and gettext

RUN rm /usr/local/etc/php-fpm.d/*
COPY php-fpm.d/www.conf /usr/local/etc/php-fpm.d/
#RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
COPY php.ini /usr/local/etc/php
