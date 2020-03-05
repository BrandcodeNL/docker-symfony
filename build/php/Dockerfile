FROM php:7.2-fpm

RUN apt-get update && apt-get install libzip-dev -y \
    && apt-get install -y git \
    && apt-get install -y libpng-dev \
    && apt-get install -y zlib1g-dev libicu-dev g++

RUN apt-get install -y build-essential libssl-dev libpng-dev libjpeg-dev libfreetype6-dev

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd
    
RUN pecl install redis \
    && pecl install zip \
    && pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "date.timezone = Europe/Amsterdam" > /usr/local/etc/php/conf.d/timezone.ini \
    && echo 'memory_limit = 1024M' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini \
    && docker-php-ext-install pdo pdo_mysql exif intl fileinfo \
    && docker-php-ext-enable redis zip exif intl fileinfo

RUN usermod -u 1000 www-data    
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

RUN curl -L https://cs.symfony.com/download/php-cs-fixer-v2.phar -o php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

RUN ln -snf /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

WORKDIR /var/www/html/symfony

RUN usermod -u 1000 www-data
