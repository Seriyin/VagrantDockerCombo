# Use an official PHP image as a parent image
FROM php:7.1


# Install any needed packages
RUN buildDeps=" \
        libpng12-dev \
        libjpeg-dev \
        libmcrypt-dev \
        curl \
        python \
        python3 \
        libxml2-dev \
        zlib1g-dev \
        libicu-dev \
        librecode0 \
        mysql-client \
        libxslt-dev \
        libmcrypt-dev \
        libfreetype6-dev \
        libpq-dev \
        libcurl3-dev \
        libcurl3 \
        libpspell-dev \
        aspell-en \
        g++ \
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
    iconv \
    xml \
    hash \
    curl \
    phar \
    intl \
    json \
    xsl \
    pdo_mysql \
    mysqli \
    bcmath \
    soap \
    zip \
    && apt-get autoremove -y

COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm.conf /usr/local/etc/
COPY auth.json ./
COPY composer.json ./

# Make sure the volume mount point is empty
RUN rm -rf /var/www/html/*

# Install magento from composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer
    
RUN composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition /var/www/html/web && \
    cd /var/www/html/web

RUN find var generated vendor pub/static pub/media app/etc -type f -exec chmod u+w {} \; && \
    find var vendor generated pub/static pub/media app/etc -type d -exec chmod u+w {} \; && \
    chmod u+x bin/magento && \
    export PATH=$PATH:/var/www/html/web/bin && \
    magento setup:install --admin-firstname=SDB --admin-lastname=SDB --admin-email=example@gmail.com --admin-user=magento1 --admin-password=magento1 --db-host=10.0.0.103 --db-name=magento1 --db-user=magento1 --db-password=magento1 --use-rewrites=1
