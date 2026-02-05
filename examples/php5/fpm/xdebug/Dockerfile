FROM michaeldelle/php:fpm
LABEL maintainer="michael@michaeldelle.com"

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN export DEBIAN_FRONTEND=noninteractive; \
    apt-get update -qq \
        && apt-get install -qqy php5-xdebug; \
    apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*;

COPY xdebug.ini /etc/php5/fpm/conf.d/20-xdebug.ini