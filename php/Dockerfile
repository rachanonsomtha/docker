FROM php:7.4-fpm

# Timezone
ENV TZ=Asia/Bangkok
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libwebp-dev \ 
    libxpm-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    libicu-dev \
    libmcrypt-dev \
    libxslt1-dev \
    unzip     

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
ENV REDIS_VERSION 4.0.2

# Get redis and latest Composer
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$REDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-* /usr/src/php/ext/redis \
    && curl -sS https://getcomposer.org/installer | \
       php -- --install-dir=/usr/local/bin --filename=composer

RUN docker-php-ext-configure \
    gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd intl soap xsl redis

COPY php/src/ /var/www/html

# Set working directory
WORKDIR /var/www/html

# Install composer based on Laravel composer.json, Change permission and Laravel generate Application Key
RUN chown -R www-data:root /var/www/html
