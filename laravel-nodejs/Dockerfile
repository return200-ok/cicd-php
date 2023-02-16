FROM node:12.22.7 as node-build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY ./ .
RUN npm run build

FROM composer AS composer-build-stage
WORKDIR /app
COPY . /app
RUN composer install \
  --ignore-platform-reqs

FROM return200/php-nginx-alpine:latest
COPY --from=node-build-stage /app/dist /app
COPY --chown=nginx --from=composer-build-stage /app /var/www/html