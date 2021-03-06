FROM php:7.3-fpm-stretch

RUN apt-get -y -qq update && \
    apt-get -y -qq install --no-install-recommends gosu git unzip libpng-dev libjpeg-dev libfreetype6-dev libicu-dev libzip-dev libxml2-dev libxslt-dev --no-install-recommends && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) pdo_mysql bcmath gd intl opcache zip soap xsl && \
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/* && \
    rm -rf /tmp/*

RUN curl -Lo /usr/bin/composer https://getcomposer.org/composer-stable.phar && \
    chmod +x /usr/bin/composer

RUN curl -sL https://deb.nodesource.com/setup_13.x | bash && \
    apt-get install -y nodejs

RUN useradd -m app

ARG XDEBUG

RUN if [ $XDEBUG ]; then pecl install xdebug && docker-php-ext-enable xdebug; fi

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["php-fpm", "-F", "-R"]