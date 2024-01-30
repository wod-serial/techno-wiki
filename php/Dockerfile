FROM php:5.6-fpm

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/


RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-install mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mbstring

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions apc

EXPOSE 9000
CMD ["php-fpm"]
