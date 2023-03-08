ARG ALPINE_VERSION=3.12
FROM alpine:${ALPINE_VERSION}

LABEL Description="Lightweight container with Nginx 1.22 & PHP 7 based on Alpine Linux."

# Setup document root
WORKDIR /var/www/html

RUN apk add --no-cache  --repository http://dl-cdn.alpinelinux.org/alpine/edge/community php

# Install curl, nginx, php, supervisord
RUN apk add --no-cache \
  curl \
  nginx \
  supervisor

# Install php-extensions
# The Laravel server requirements specify the PHP extensions, including BCMath, Ctype, cURL, DOM, Fileinfo, JSON, Mbstring, OpenSSL, PCRE, PDO, Tokenizer, and XML, are required. 
# These extensions are usually included and enabled during a PHP installation.
RUN apk add --no-cache \
  php7-ctype \
  php7-curl \
  php7-dom \
  php7-fpm \
  php7-gd \
  php7-intl \
  php7-mbstring \
  php7-mysqli \
  php7-opcache \
  php7-openssl \
  php7-phar \
  php7-session \
  php7-xml \
  php7-xmlreader \
  php7-bcmath \
  php7-tokenizer \
  php7-dev \
  php7-pear

# Install Xdebug
RUN apk add --no-cache \
    git \
    yarn \
    autoconf \
    g++ \
    make \
    openssl-dev 
RUN pecl install xdebug-3.1.6
# Configure nginx - http
COPY config/nginx.conf /etc/nginx/nginx.conf
# Configure nginx - default server
COPY config/conf.d /etc/nginx/conf.d/

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php7/php-fpm.d/www.conf
COPY config/php.ini /etc/php7/conf.d/custom.ini

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Make sure files/folders needed by the processes are accessable when they run under the nginx user
RUN chown -R nginx:nginx /var/www/html /run /var/lib/nginx /var/log/nginx

# Switch to use a non-root user from here on
USER nginx

# Add application
COPY --chown=nginx src/ /var/www/html/

# Expose the port nginx is reachable on
EXPOSE 8080

# Let supervisord start nginx & php-fpm
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Configure a healthcheck to validate that everything is up&running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
