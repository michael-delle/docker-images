FROM php:7.4-fpm-alpine AS base
LABEL maintainer="michael@michaeldelle.com"

ENV PM_MAX_CHILDREN=100
ENV PM_START_SERVERS=10
ENV PM_MIN_SPARE_SERVERS=5
ENV PM_MAX_SPARE_SERVERS=15
ENV PM_MAX_REQUESTS=1000

COPY shared/run-php-fpm /usr/local/bin/run-php-fpm

RUN apk add --no-cache \
        freetype \
        git \
        icu-libs \
        imagemagick-libs \
        libjpeg-turbo \
        libmcrypt \
        libpng \
        libxml2 \
        libzip \
        procps \
        unzip \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        freetype-dev \
        icu-dev \
        imagemagick-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libxml2-dev \
        libzip-dev \
        linux-headers \
        zlib-dev \
    && pecl install imagick-3.7.0 \
    && pecl install mcrypt-1.0.7 \
    && docker-php-ext-configure intl \
    && docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
    && docker-php-ext-configure opcache \
    && docker-php-ext-configure zip \
    && docker-php-ext-install -j"$(nproc)" \
        gd \
        intl \
        opcache \
        mysqli \
        soap \
        zip \
    && docker-php-ext-enable \
        imagick \
        mcrypt \
    && apk del .build-deps \
    && rm -rf /tmp/* /var/cache/apk/* \
    && chmod a+x /usr/local/bin/run-php-fpm

COPY prod/php-fpm.conf /usr/local/etc/php-fpm.d/zz-php.conf
COPY shared/php.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini

CMD ["/usr/local/bin/run-php-fpm"]

# Development stage
FROM composer:2 AS composer
FROM base AS dev

RUN apk add --no-cache --virtual .xdebug-deps $PHPIZE_DEPS linux-headers \
    && pecl install xdebug-3.1.6 \
    && docker-php-ext-enable xdebug \
    && apk del .xdebug-deps \
    && rm -rf /tmp/* /var/cache/apk/*; \
    mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

COPY --from=composer /usr/bin/composer /usr/bin/composer
COPY dev/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY dev/php-dev.ini /usr/local/etc/php/conf.d/opcache-dev.ini

# Production stage
FROM base AS prod

COPY prod/php.ini /usr/local/etc/php/conf.d/opcache-prod.ini
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"