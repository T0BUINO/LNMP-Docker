FROM php:8.1.25-fpm-bullseye

RUN buildDeps='libmemcached-dev libssl-dev zlib1g-dev libfreetype-dev libjpeg62-turbo-dev libpng-dev libwebp-dev' \
    && apt update && apt install -y $buildDeps \
    && docker-php-ext-install mysqli \
    && pecl install memcached-3.2.0  \ 
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && docker-php-ext-install -j$(nproc) gd\
    && docker-php-ext-install opcache \
    && docker-php-ext-enable mysqli memcached gd opcache\
    && groupadd www \
    && useradd -g www -M -s /sbin/nologin www

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp 

    # Line 6: Noted that the "pcel extensions" should be installed in series. e.g. "pecl install memcached-3.2.0 && pecl install redis-5.3.7".
    # Line 7: The PNG support might be a built-in fucntion for GD. >> https://github.com/docker-library/php/issues/912#issuecomment-559918036