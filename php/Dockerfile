FROM php:8.1-fpm-alpine

RUN docker-php-ext-install pdo pdo_mysql

# allow super user - set this if you use Composer as a
# super user (root) at all times like in docker containers
ENV COMPOSER_ALLOW_SUPERUSER=1

#obtain composer using multi-stage build
# https://docs.docker.com/build/building/multi-stage/
COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

# Here, we are copying only composer.json and composer.lock files (instead of the whole project) 
# right before doing composer install.
# This is enough to take advantage of Docker’s cache mechanism and composer install will be executed only if
# composer.json or composer.lock files are changed.
# https://medium.com/@softius/faster-docker-builds-with-composer-install-b4d2b15d0fff
COPY ./app/composer.* ./

# install
RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

# copy application files to the working directory
COPY ./app .

# run composer dump-autoload --optimize
RUN composer dump-autoload --optimize