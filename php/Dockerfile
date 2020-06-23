FROM php:7.4-fpm-alpine
RUN cd ~ \
    && wget -c https://getcomposer.org/composer-stable.phar \
    && mv composer-stable.phar /usr/bin/composer \
    && chmod a+x /usr/bin/composer \
    && sed -i 's;http://dl-cdn.alpinelinux.org;https://mirrors.tuna.tsinghua.edu.cn;g' /etc/apk/repositories; \
    \
    set -ex; \
    \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS vim openssl-dev freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev libxpm-dev zlib-dev libzip-dev; \
    \
    docker-php-ext-configure gd --with-freetype --with-jpeg; \
    docker-php-ext-configure zip; \
    docker-php-ext-install gd pdo_mysql mysqli zip opcache; \
    docker-php-ext-install bcmath; \
    wget -c http://pecl.php.net/get/mongodb-1.6.1.tgz; \
    wget -c http://pecl.php.net/get/igbinary-3.0.1.tgz; \
    wget -c http://pecl.php.net/get/redis-5.1.1.tgz; \
    pecl install mongodb-1.6.1.tgz; \
    pecl install igbinary-3.0.1.tgz; \
    pecl install redis-5.1.1.tgz; \
    docker-php-ext-enable mongodb igbinary redis; \
    rm -f mongodb-1.6.1.tgz; \
    rm -f igbinary-3.0.1.tgz; \
    rm -f redis-5.1.1.tgz; \
    runDeps="$( \
        scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
            | tr ',' '\n' \
            | sort -u \
            | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )"; \
    apk add --virtual .phpexts-rundeps $runDeps; \
    apk del .build-deps
