FROM php:8.3-fpm-alpine AS base

USER root

WORKDIR /app

VOLUME /app/var/

RUN apk add --no-cache \
	acl \
	file \
	gettext \
	git \
    zip \
    libzip-dev \
    icu-dev

RUN docker-php-ext-install \
  intl \
  opcache \
  zip

RUN curl -s https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER=1

FROM base AS dev
ENV APP_ENV=dev XDEBUG_MODE=off

RUN	mkdir -p var/cache var/log && chmod a+w var/cache var/log


# Prod image
FROM base AS prod

ENV APP_ENV=prod

COPY ./app ./

RUN composer install --no-cache --prefer-dist --no-dev --no-autoloader --no-scripts --no-progress

RUN	mkdir -p var/cache var/log && chmod a+w var/cache var/log; \
  composer dump-autoload --classmap-authoritative --no-dev; \
  composer dump-env prod; \
  composer run-script --no-dev post-install-cmd; \
  chmod +x bin/console; sync;
