ARG ALPINE_VERSION=3.8
FROM alpine:${ALPINE_VERSION} as aerospike-c-builder
# ENV AS_C_VERSION=4.6.18
ENV AS_C_VERSION=4.0.7

WORKDIR /src

RUN apk update
RUN apk add --no-cache \
    build-base \
    bash linux-headers \
    libuv libuv-dev \
    openssl openssl-dev \
    lua5.1 lua5.1-dev zlib-dev
RUN wget https://artifacts.aerospike.com/aerospike-client-c/${AS_C_VERSION}/aerospike-client-c-src-${AS_C_VERSION}.zip \
    && unzip aerospike-client-c-src-${AS_C_VERSION}.zip \
    && mv aerospike-client-c-src-${AS_C_VERSION} aerospike-client-c \
    && cd aerospike-client-c \
    && make EVENT_LIB=libuv



# Stage 2: Build Aerospike PHP client
FROM php5.6:alpine3.8 as aerospike-php-builder

WORKDIR /aerospike-client-php5
COPY aerospike-client-php5 /aerospike-client-php5

COPY --from=aerospike-c-builder /src/aerospike-client-c/target/Linux-x86_64/ /aerospike-client-php5/src/aerospike-client-c/

# USER root
# RUN apk add bash

# RUN cd src/aerospike && ./build.sh

