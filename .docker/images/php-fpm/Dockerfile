FROM php:7.1-fpm

# replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update && apt-get install -y \
    git \
    zip

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install -j$(nproc) \
    opcache \
    pdo_mysql \
    zip

RUN apt-get install -y \
        zlib1g-dev \
        libicu-dev \
        g++ \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

RUN pecl install xdebug-2.5.0 \
    && docker-php-ext-enable xdebug

RUN apt-get install -y \
    libtidy-dev \
    && docker-php-ext-install -j$(nproc) tidy

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 6.10.3

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

EXPOSE 9000

CMD ["php-fpm", "-F"]
