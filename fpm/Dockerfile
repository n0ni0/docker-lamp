FROM php:7.1-fpm
RUN apt-get update && apt-get install -y \
	sudo \
	git \
	bzip2 \
	cron \
	wget \
	libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libxml2-dev \
        libcurl4-openssl-dev \
	libxslt-dev \
	libicu-dev \
	nano \
	vim \
    gnupg \
    && docker-php-ext-install -j$(nproc) iconv mcrypt curl dom hash pdo pdo_mysql mysqli simplexml soap xsl intl zip opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# msmtp
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y msmtp

# mysql-client
RUN apt-get update && apt-get install -y default-mysql-client && rm -rf /var/lib/apt

# Xdebug
RUN pecl install xdebug-2.5.0 \
  &&  docker-php-ext-enable xdebug
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
RUN chmod 777 /usr/local/etc/php/conf.d/xdebug.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Node.js
RUN curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
RUN apt-get install -y nodejs

# Gulp
RUN npm install --global gulp
