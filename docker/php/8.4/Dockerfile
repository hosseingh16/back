FROM php:8.4-fpm-alpine

LABEL maintainer="Hossein"

# -----------------------------
# System dependencies
# -----------------------------
RUN apk add --no-cache \
    bash \
    git \
    curl \
    zip \
    unzip \
    icu-dev \
    oniguruma-dev \
    libxml2-dev \
    libzip-dev \
    linux-headers \
    autoconf \
    gcc \
    g++ \
    make \
    openssl-dev \
    tzdata

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
# Composer (latest)
# -----------------------------
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# -----------------------------
# Timezone
# -----------------------------
RUN cp /usr/share/zoneinfo/UTC /etc/localtime \
    && echo "UTC" > /etc/timezone

# -----------------------------
# Create user = hossein (UID 1000)
# -----------------------------
RUN addgroup -g 1000 hossein \
    && adduser -u 1000 -G hossein -g hossein -s /bin/sh -D hossein

WORKDIR /var/www/html

USER hossein

EXPOSE 9000
