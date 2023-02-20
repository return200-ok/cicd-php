ARG ALPINE_VERSION=3.8
FROM alpine:${ALPINE_VERSION}

LABEL Description="Lightweight container with Nginx 1.22 & PHP 7 based on Alpine Linux."

# Setup document root
WORKDIR /var/www/html


# Install curl, nginx, php, supervisord
RUN apk add --no-cache \
  php \
  curl \
  nginx \
  supervisor

# Install php-extensions
# The Laravel server requirements specify the PHP extensions, including BCMath, Ctype, cURL, DOM, Fileinfo, JSON, Mbstring, OpenSSL, PCRE, PDO, Tokenizer, and XML, are required. 
# These extensions are usually included and enabled during a PHP installation.
RUN apk add --no-cache \
  php5-ctype \
  php5-curl \
  php5-dom \
  php5-fpm \
  php5-phar \
  php5-gd \
  php5-intl \
  php5-mysqli \
  php5-opcache \
  php5-openssl \
  php5-phar \
  php5-xml \
  php5-xmlreader \
  php5-bcmath 

# Configure nginx - http
COPY config/nginx.conf /etc/nginx/nginx.conf
# Configure nginx - default server
COPY config/conf.d /etc/nginx/conf.d/

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php5/php-fpm.d/www.conf
COPY config/php.ini /etc/php5/conf.d/custom.ini

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Make sure files/folders needed by the processes are accessable when they run under the nginx user
RUN chown -R nginx:nginx /var/www/html /run /var/lib/nginx /var/log

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
