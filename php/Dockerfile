FROM php:alpine AS cacert

FROM php:7.4-fpm-alpine

 # сертификаты в старом имадже протухли, так что воруем их из более нового

COPY --from=cacert /etc/ssl /root/ssl

RUN mv /etc/ssl/openssl.cnf* /root/ssl && rm -rf /etc/ssl && mv /root/ssl /etc/ssl \
  && pear update-channels \
  && pear install Mail Mail_Mime Net_SMTP

RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev libwebp-dev \
    && docker-php-ext-configure gd --enable-gd --with-webp --with-jpeg --with-freetype \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN apk add --no-cache --virtual .build-deps gettext-dev \
    && docker-php-ext-install -j$(nproc) gettext \
    && apk del -f .build-deps

RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libzip-dev imagemagick-dev libtool \
    && apk add --no-cache imagemagick mysql-client \
    && pecl install imagick  \
    && docker-php-ext-enable imagick \
    && rm -rf /tmp/pear \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysql mysqli zip \
    && apk del -f .build-deps

RUN apk add --no-cache libmemcached-libs zlib \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS zlib-dev libmemcached-dev \
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
    && apk del -f .build-deps

RUN set -ex \
    && apk --no-cache add icu icu-dev icu-libs \
    && docker-php-ext-install -j$(nproc) intl \
    && apk del icu-dev

RUN rm /usr/local/etc/php-fpm.d/*
COPY php-fpm.d/www.conf /usr/local/etc/php-fpm.d/
#RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
COPY php.ini /usr/local/etc/php
