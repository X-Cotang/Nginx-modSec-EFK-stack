FROM php:7.3-fpm

RUN apt update

RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql

EXPOSE 9000

# install gd 
RUN apt-get update -y && apt-get install -y libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev libfreetype6-dev  zlib1g-dev 
RUN docker-php-ext-install mbstring
RUN apt-get install -y libzip-dev
RUN docker-php-ext-install zip
RUN docker-php-ext-install gd
 

CMD ["php-fpm"]

#  allow_url_include: enable /usr/local/etc/php/php.ini copy devolop to ini