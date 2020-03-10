FROM php:7.2-apache
COPY ./apache/000-default.conf /etc/apache2/sites-available/000-default.conf
# RUN sed -i 's/\/html/\/html\/web/g'  /etc/apache2/sites-available/000-default.conf
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN a2enmod rewrite
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
# Tools
    apt-utils \
    wget \
    git \
    nano \
    iputils-ping \
    locales \
    imagemagick \
    graphicsmagick \
    ghostscript \
# Configure PHP
    libxml2-dev libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache && \
    echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmemory_limit = 512M\nmax_input_vars = 1500\nupload_max_filesize = 200M\npost_max_size = 300M' > /usr/local/etc/php/conf.d/typo3.ini && \
# Configure bash
    echo 'export LS_OPTIONS="--color=auto"\nalias ls="ls $LS_OPTIONS"\nalias ll="ls $LS_OPTIONS -aGFlh"\nalias l="ls $LS_OPTIONS -FG"' > ~/.bashrc && \
# Configure Apache & clean
    apt-get clean && \
    apt-get -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev && \
    rm -rf /var/lib/apt/lists/* /usr/src/*
# ======= composer =======
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
COPY ./src /var/www/html/
RUN composer install

EXPOSE 80