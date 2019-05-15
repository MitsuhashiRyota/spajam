# Docker Image
FROM php:7.2.0-fpm

#ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --fix-missing \
    apt-utils \
    gnupg

# Library Install
RUN echo "deb http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list
RUN echo "deb-src http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list
RUN curl -sS --insecure https://www.dotdeb.org/dotdeb.gpg | apt-key add -

RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libzip-dev

RUN docker-php-ext-install zip pdo_mysql

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

# Composer Install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer

# Composer Permission
ENV COMPOSER_ALLOW_SUPERUSER 1

# Composer Install Package
ENV COMPOSER_HOME /composer

# Composer Global Path
ENV PATH $PATH:/composer/vendor/bin

# Install Laravel PHP
RUN composer global require "laravel/installer"

# Node JS Install
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \ 
    && apt-get update \
    && apt-get install -y nodejs

# Work Directory
WORKDIR /src
