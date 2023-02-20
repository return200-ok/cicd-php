ARG ALPINE_VERSION=3.8
FROM alpine:${ALPINE_VERSION} as aerospike-c-builder
ENV AS_C_VERSION=4.6.18
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