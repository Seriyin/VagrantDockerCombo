# Use an official Ubuntu image as a parent image
FROM php:7.1

WORKDIR ~/

# Install any needed packages
RUN buildDeps=" \
        libpng12-dev \
        libjpeg-dev \
        libmcrypt-dev \
        libxml2-dev \
        freetype* \
    "; \
    set -x \
    && apt-get update && apt-get install -y $buildDeps --no-install-recommends && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure \
    gd --with-png-dir=/usr --with-jpeg-dir=/usr --with-freetype-dir \
    && docker-php-ext-install \
    gd \
    mbstring \
    opcache \
    mcrypt \
    curl \
    iconv \
    xml \
    intl \
    common \
    cgi \
    json \
    odbc \
    xsl \
    fpm \
    mysql \
    pdo_mysql \
    bcmath \
    soap \
    zip \
    && apt-get auto-remove -y

COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm.conf /usr/local/etc/

# Make sure the volume mount point is empty
RUN rm -rf /var/www/html/*

# Install magerun
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition /var/www/html/ && \
    apt-get update && \
    apt-get install -qy mysql-client

# Make port 8080 available to the world outside this container
EXPOSE 80
EXPOSE 443
