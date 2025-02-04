FROM php:8.3-fpm

COPY composer.lock composer.json /var/www/

WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    build-essential \
    libzip-dev \
    libpng-dev \
    libpq-dev \
    libcurl4-openssl-dev \
    libxml2-dev \
    libcap2-bin \
    libonig-dev \
    libjpeg62-turbo-dev \
    libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev \
    libfreetype6 \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    libonig-dev \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install -j$(nproc) \
    iconv \
    mbstring \
    pdo \
    pdo_pgsql \
    xml \
    soap \
    zip \
    opcache \
    curl

RUN pecl install redis

RUN docker-php-ext-enable \
    redis \
    curl

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN groupadd --force -g 1000 www
RUN useradd -u 1000 --no-user-group -ms /bin/bash -g www www

COPY . /var/www

COPY --chown=www:www . /var/www

USER www

EXPOSE 9000
CMD ["php-fpm"]
