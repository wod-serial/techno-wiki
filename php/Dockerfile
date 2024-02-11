FROM php:alpine AS cacert

FROM php:5.6-fpm-alpine

 # сертификаты в старом имадже протухли, так что воруем их из более нового

COPY --from=cacert /etc/ssl /root/ssl

RUN mv /etc/ssl/openssl.cnf* /root/ssl && rm -rf /etc/ssl && mv /root/ssl /etc/ssl \
  && pear update-channels \
  && pear upgrade && pear install Mail Mail_Mime

RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN apk add --no-cache --virtual .build-deps gettext-dev \
    && docker-php-ext-install -j$(nproc) gettext \
    && apk del -f .build-deps

RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libzip-dev imagemagick-dev libtool \
    && apk add --no-cache imagemagick mysql-client \
    && pecl install imagick &&  \
    && docker-php-ext-enable imagick \
    && rm -rf /tmp/pear \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysql mysqli zip \
    && apk del -f .build-deps

RUN apk add --no-cache --virtual .build-deps g++ make autoconf \
    && pecl install -o -f redis-4.3.0 \
    && docker-php-ext-enable redis \
    && rm -rf /tmp/pear \
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
