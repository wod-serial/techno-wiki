FROM php:5.6-fpm
FROM php:alpine AS cacert

RUN apk update && apk add --no-cache imagemagick-dev libtool make g++

RUN wget https://github.com/ImageMagick/ImageMagick6/archive/refs/tags/6.9.12-28.tar.gz && \
     tar -xzf 6.9.12-28.tar.gz && \
     cd ImageMagick6-6.9.12-28 && \
     ./configure && \
     make && \
     make install

RUN wget https://pecl.php.net/get/imagick-3.4.4.tgz && \
     tar -xzf imagick-3.4.4.tgz && \
     cd imagick-3.4.4 && \
     phpize && \
     ./configure --with-imagick=/usr/local && \
     make && \
     make install

RUN pecl install imagick;

RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-install mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mbstring


RUN echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini

EXPOSE 9000
CMD ["php-fpm"]
