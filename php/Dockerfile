FROM php:7.3-fpm

# Install dependencies
RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -yq \
    apt-utils \
    build-essential \
    dialog \
    curl \
    git \
    gnupg \
    g++ \
    jpegoptim optipng pngquant gifsicle \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libicu-dev \
    libpng-dev \
    libssl-dev\
    libzip-dev \
    locales \
    mariadb-client \
    unzip \
    vim \
    wget \
    zip

# Install nodejs
RUN  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash \
&& export NVM_DIR="$HOME/.nvm" \
&& [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
&& nvm install node && nvm use node \
&& npm cache clean -f && npm install -g n && n stable && npm install cross-env

# Install php extensions
RUN docker-php-ext-install \
    bcmath /
    ctype /
    curl /
    dom /
    exif /
    gd /
    iconv /
    intl /
    json /
    mbstring /
    mysqli /
    pcntl /
    opcache /
    openssl /
    pcre /
    pdo /
    pdo_mysql /
    soap /
    simplexml /
    spl /
    tokenizer /
    xml /
    xmlrpc /
    zip /
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# Install composer
RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer && \
    composer self-update --preview

# Install redis extension
RUN pecl install redis-4.1.1 && \
    docker-php-ext-enable redis

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