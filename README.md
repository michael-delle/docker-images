# Docker Images

[![Docker Image CI](https://github.com/michael-delle/docker-images/actions/workflows/docker-image.yml/badge.svg?branch=master&event=push)](https://github.com/michael-delle/docker-images/actions/workflows/docker-image.yml)

## examples

1. cloudbuild.yaml - Cloud Build CI configuration used by example builds.
2. openresty/ - OpenResty example with custom/Dockerfile and README.md for building a custom image.
3. php5/ - PHP 5 FPM example: fpm/Dockerfile, fpm/php-fpm.conf, fpm/run-php5-fpm and an Xdebug variant in fpm/xdebug/
   with Dockerfile and xdebug.ini.
4. php72-appengine-build/ - App Engine build image with Dockerfile for PHP 7.2 build workflows.
5. php74/ - PHP 7.4 FPM example containing Dockerfile, development Xdebug config dev/xdebug.ini, and production configs
   prod/php.ini, prod/php-fpm.conf, prod/run-php-fpm.
6. php82/ - PHP 8.2 example with Dockerfile and production config files in prod/ (prod/php.ini, prod/php-fpm.conf,
   prod/run-php-fpm).

Usage: build the Dockerfile in the appropriate example folder, mount application code into `/var/www/html` and link a
web server/reverse proxy to PHP-FPM (commonly port 9000).

## michaeldelle-com

A lightweight Docker image providing PHP 7.4 with PHP-FPM, intended for web applications. The repository contains a
Dockerfile plus separate development and production configuration stacks:

* Development: dev/xdebug.ini (Xdebug configuration).
* Production: prod/php.ini, prod/php-fpm.conf, and prod/run-php-fpm (PHP-FPM startup script).
* Buildable via the Dockerfile in the directory.
* Intended to run application code mounted into /var/www/html and to accept connections on the PHP-FPM socket/port (
  commonly 9000).
* Configuration is customizable by mounting or replacing the files in dev/ or prod/ as needed for your environment.