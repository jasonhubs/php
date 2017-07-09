## php-fpm with extension "ssh2"

#### describe:

you can use the Dockerfile to create a php image with extension ssh2.

#### Dockerfile

``` Dockerfile
FROM php:7.1-fpm
MAINTAINER jczhangmail@126.com

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libssh2-1-dev \ 
        libssh2-php \ 
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install mbstring pdo pdo_mysql zip opcache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN echo "cgi.fix_pathinfo=0" >> /usr/local/etc/php/php.ini
RUN pecl channel-update pecl.php.net
#if php version <<6 >>4 ssh2-1.0 to ssh2-0.12
RUN pecl install -a ssh2-1.0
RUN php5enmod ssh2
RUN echo "extension=ssh2.so" >> /usr/local/etc/php/php.ini
WORKDIR /var/www
```
