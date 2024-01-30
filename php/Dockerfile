FROM ubuntu:18.04


RUN apt-get update \
    && apt-get install -y software-properties-common \
    && apt-get update \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata \
    && apt-get install -y php5.6 php-fpm php-pear \
    && apt-get install -y php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-xml

RUN apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions gd mysql mcrypt mbstring xml imagick

EXPOSE 9000
