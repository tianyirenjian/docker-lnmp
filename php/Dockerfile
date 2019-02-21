FROM php:7.2-fpm-alpine
RUN cd ~ \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/bin php --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && chmod a+x /usr/bin/composer;\
    sed -i 's;http://dl-cdn.alpinelinux.org;https://mirrors.tuna.tsinghua.edu.cn;g' /etc/apk/repositories; \
    \
    set -ex; \
    \
    apk add --no-cache --virtual .build-deps freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev libxpm-dev zlib-dev libzip-dev; \
    \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include --with-jpeg-dir=/usr/include --with-webp-dir=/usr/include --with-png-dir=/usr/include --with-xpm-dir=/usr/include; \
    docker-php-ext-configure zip --with-libzip; \
    docker-php-ext-install gd pdo_mysql mysqli zip opcache; \
    runDeps="$( \
        scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
            | tr ',' '\n' \
            | sort -u \
            | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )"; \
    apk add --virtual .phpexts-rundeps $runDeps; \
    apk del .build-deps
