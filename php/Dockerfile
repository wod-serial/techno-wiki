FROM ubuntu:18.04


RUN apt-get update \
&& apt-get -y install lsb-release ca-certificates curl \
&& curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg \
&& sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list' \
&& add-apt-repository ppa:ondrej/php apt-get update \
    && apt-get install -y software-properties-common \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata \
    && apt-get install -y php5.6 php-fpm php-pear php5-dev

RUN apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions gd mysql mcrypt mbstring xml imagick

EXPOSE 9000
