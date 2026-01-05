FROM php:8.5-fpm-alpine

LABEL maintainer="Hossein"

# -----------------------------
# System + build dependencies
# -----------------------------
RUN apk add --no-cache \
    bash \
    git \
    curl \
    zip \
    unzip \
    tzdata \
    icu-dev \
    oniguruma-dev \
    libxml2-dev \
    libzip-dev \
    openssl-dev \
    linux-headers \
    $PHPIZE_DEPS

# -----------------------------
# PHP Extensions
# -----------------------------
RUN docker-php-ext-configure intl \
    && docker-php-ext-install \
        pdo \
        pdo_mysql \
        mysqli \
        intl \
        mbstring \
        xml \
        zip \
        bcmath \
        opcache \
        pcntl \
        sockets

# -----------------------------
# Redis
# -----------------------------
RUN pecl install redis \
    && docker-php-ext-enable redis

# -----------------------------
# Xdebug (DEV)
# -----------------------------
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# -----------------------------
# PHP config
# -----------------------------
COPY php.ini /usr/local/etc/php/php.ini

# -----------------------------
# Composer
# -----------------------------
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# -----------------------------
# Timezone
# -----------------------------
RUN cp /usr/share/zoneinfo/UTC /etc/localtime \
    && echo "UTC" > /etc/timezone

# -----------------------------
# Workdir
# -----------------------------
WORKDIR /var/www/html

# -----------------------------
# User (Laravel-friendly)
# -----------------------------
RUN addgroup -g 1000 laravel \
    && adduser -G laravel -g laravel -s /bin/sh -D laravel \
    && chown -R laravel:laravel /var/www/html

USER laravel

EXPOSE 9000
