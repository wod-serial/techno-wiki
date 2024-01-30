FROM php:5.6-fpm
FROM php:alpine AS cacert

COPY --from=cacert /etc/ssl /root/ssl

RUN mv /etc/ssl/openssl.cnf* /root/ssl && rm -rf /etc/ssl && mv /root/ssl /etc/ssl \
  && pear update-channels \
  && pear upgrade

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
