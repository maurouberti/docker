FROM php:8.3-fpm-alpine

WORKDIR /var/www

RUN apk update && \
    apk upgrade && \
    apk add --no-cache curl \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

EXPOSE 9000
CMD ["php-fpm"]
