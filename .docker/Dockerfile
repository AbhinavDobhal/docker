FROM php:7.1.5-apache

# Install PHP extensions
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libcurl4-openssl-dev \
        locales \
        git \
        openssl \
        openssh-client \
        librabbitmq-dev \
        pkg-config \
        net-tools \
        libmagickwand-dev \
        libmagickcore-dev \
        libssl-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        unzip \
        make \
        php-pear \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) pdo_mysql json bcmath intl opcache mbstring xml zip



# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Put apache config for Laravel
COPY apache2-laravel.conf /etc/apache2/sites-available/laravel.conf
COPY server.crt /etc/apache2/ssl/server.crt
COPY server.key /etc/apache2/ssl/server.key
RUN a2dissite 000-default.conf && a2ensite laravel.conf && a2enmod rewrite
RUN a2enmod ssl
RUN service apache2 restart
# Change uid and gid of apache to docker user uid/gid
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
WORKDIR /var/www/html
