FROM composer AS build-stage

WORKDIR /app

# copying the source directory and install the dependencies with composer
COPY . /app

# run composer install to install the dependencies
RUN composer install \
  --ignore-platform-reqs

# continue stage build with the desired image and copy the source including the
# dependencies downloaded by composer
FROM return200/php-nginx-alpine:latest
COPY --chown=nginx --from=build-stage /app /var/www/html