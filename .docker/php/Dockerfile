FROM php:7.2-apache

MAINTAINER Nicolas Sicard <nicolas.sicard@mandarine.academy>

WORKDIR /var/www/html

RUN apt-get update && apt-get install -y \
        apt-transport-https \
        zlib1g-dev \
        gnupg2 \
        git \
        curl \
        build-essential \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-install -j$(nproc) \
        zip \
        pdo_mysql

# Node.js
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs

# Yarn install
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y --no-install-recommends yarn

# Apache configuration
COPY ./conf/httpd.conf /usr/local/apache2/conf/httpd.conf
RUN mkdir /usr/local/apache2/conf/sites-enabled
COPY ./conf/default.conf /usr/local/apache2/conf/sites-enabled/default.conf

# Composer installation
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN usermod -u 1000 www-data

