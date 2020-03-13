FROM php:7.4-fpm

# Install dependencies
RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -yq \
    apt-utils \
    build-essential \
    dialog \
    curl \
    default-libmysqlclient-dev \
    gifsicle \
    git \
    gnupg \
    g++ \
    jpegoptim \
    libbz2-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libicu-dev \
    libpng-dev \
    libsasl2-dev \
    libssl-dev\
    libxml2-dev \
    libzip-dev \
    locales \
    mariadb-client \
    optipng \
    pngquant \
    unzip \
    vim \
    wget \
    zip

# Install nodejs
RUN  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh | bash \
&& export NVM_DIR="$HOME/.nvm" \
&& [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
&& nvm install node && nvm use node \
&& npm cache clean -f && npm install -g n && n stable && npm install cross-env

# Install php extensions
RUN docker-php-ext-install bcmath bz2 calendar iconv intl mysqli opcache pdo_mysql soap xmlrpc zip \
    # && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    # && docker-php-ext-install gd exif
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

# Install PECL extensions
RUN pecl install redis-5.1.1 \
    && pecl install xdebug-2.8.1 \
    && docker-php-ext-enable redis xdebug

# Install composer
RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer && \
    composer self-update --preview

# Add user for applications
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Cleanup
RUN apt-get autoremove && apt-get autoclean && apt-get clean

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]