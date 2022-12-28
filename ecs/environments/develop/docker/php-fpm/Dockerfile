ARG PHP_VERSION=7.3
FROM php:${PHP_VERSION}-fpm-alpine

ARG ENVIRONMENT="local" // CI/CDで上書きする [develop | staging | production]
ENV ENVIRONMENT=${ENVIRONMENT}
ENV TZ Asia/Tokyo


RUN set -eux \
  && apk add --update --no-cache git autoconf g++ libtool make libzip-dev libpng-dev libjpeg-turbo-dev freetype-dev mysql-client redis \
  && pecl install redis \
  && docker-php-ext-configure gd \
  && --with-freetype-dir=/usr/include/ \
  && --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-configure opcache --enable-opcache \
  && docker-php-ext-install opcache bcmath pdo_mysql mysqli mbstring exif zip \
  && docker-php-ext-enable redis \
  && apk del autoconf g++ libtool make \
  && rm -rf /tmp/*

COPY ./ecs/environments/${ENVIRONMENT}/docker/php-fpm/conf/zzz-.conf /usr/local/etc/php-fpm.d/zzz-.conf
COPY ./ecs/environments/${ENVIRONMENT}/docker/php-fpm/conf/php.ini /usr/local/etc/php/php.ini

COPY ./ /var/www/app/
WORKDIR /var/www/app


# Composer のインストール g
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
  && COMPOSER_MEMORY_LIMIT=-1 /usr/bin/composer install --no-dev

RUN php artisan view:clear \
    && php artisan cache:clear \
    && php artisan config:clear

# nginxコンテナに共有する
RUN mkdir -p /var/run/php-fpm
VOLUME ["/var/run/php-fpm"]

# VOLUMEでマウントするとroot権限になるので権限設定
RUN chown -R www-data:www-data storage \
    && chown -R www-data:www-data bootstrap/cache \
    && chmod 2755 storage \
    && chown -R www-data:www-data /var/www/app
