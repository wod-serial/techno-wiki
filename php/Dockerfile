FROM php:5.6-fpm

RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-install mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install imagick && \
    docker-php-ext-enable imagick

EXPOSE 9000
CMD ["php-fpm"]
