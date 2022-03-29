FROM php:8.1.4-fpm-alpine3.15

RUN apk add --no-cache shadow openssl bash mysql-client libxml2-dev zlib-dev libzip-dev \
	    freetype-dev libpng-dev libjpeg-turbo-dev\
	    oniguruma-dev icu-dev g++ \
        gnu-libiconv \
            $PHPIZE_DEPS

RUN docker-php-ext-install mbstring pdo pdo_mysql mysqli xml zip simplexml sockets intl exif
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

RUN touch /home/www-data/.bashrc | echo "PS1='\w\$ '" >> /home/www-data/.bashrc

ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN usermod -u 1000 www-data

WORKDIR /var/www

RUN rm -rf /var/www/html && ln -s public html

USER www-data

EXPOSE 9000

ENTRYPOINT ["php-fpm"]
