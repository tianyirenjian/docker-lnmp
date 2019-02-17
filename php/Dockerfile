from php:7.2-fpm
copy sources.list /etc/apt/sources.list
run cd ~ \
    && apt-get update -y && apt-get install libpng-dev \
    && docker-php-ext-install pdo_mysql mysqli gd