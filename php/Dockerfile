FROM php:5.6-fpm

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/


RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-install mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mbstring

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

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

RUN echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini

EXPOSE 9000
CMD ["php-fpm"]
