FROM php:7.4-fpm AS base
LABEL maintainer="michael@michaeldelle.com"

ENV DEBIAN_FRONTEND=noninteractive
ENV PM_MAX_CHILDREN=100
ENV PM_START_SERVERS=10
ENV PM_MIN_SPARE_SERVERS=5
ENV PM_MAX_SPARE_SERVERS=15
ENV PM_MAX_REQUESTS=1000

COPY prod/run-php-fpm /run-php-fpm

RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS"; \
    apt-get update -qq \
        && apt-get install -qqy --no-install-recommends \
            build-essential \
            git \
            libicu-dev \
            libzip-dev \
            libxml2-dev \
            unzip \
            zip \
            libmagickwand-dev \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
            pkg-config \
            libmcrypt-dev \
            zlib1g-dev \
            procps \
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
            mcrypt; \
    apt-get clean \
        && rm -rf \
          /var/lib/apt/lists/* \
          /tmp/*  \
          /var/tmp/* \
          /var/cache/apt/*; \
    chmod a+x /run-php-fpm;

COPY prod/php-fpm.conf /usr/local/etc/php-fpm.d/zz-php.conf
COPY prod/php.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini

CMD ["/run-php-fpm"]

# Development stage
FROM composer:2.9 AS composer
FROM base AS dev

RUN pecl install xdebug-3.1.6 \
        && docker-php-ext-enable \
            xdebug; \
    apt-get clean \
        && rm -rf \
          /var/lib/apt/lists/* \
          /tmp/*  \
          /var/tmp/* \
          /var/cache/apt/*; \
    mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

COPY --from=composer /usr/bin/composer /usr/bin/composer
COPY dev/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Production stage
FROM base AS prod

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini";