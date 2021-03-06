# Composer Docker Container
# Base Dockerfile: composer/base-alpine
FROM php:7.0-alpine
MAINTAINER Rob Loach <robloach@gmail.com>

# Packages
RUN apk --update add \
    autoconf \
    build-base \
    curl \
    git \
    subversion \
    freetype-dev \
    libjpeg-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libbz2 \
    libstdc++ \
    libxslt-dev \
    openldap-dev \
    make \
    php-pear \
    unzip && \
    docker-php-ext-install mcrypt zip bz2 mbstring xsl && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd && \
    docker-php-ext-configure ldap --with-libdir=lib/ && \
    docker-php-ext-install ldap && \
    apk del build-base && \
    rm -rf /var/cache/apk/*

# Memory Limit
RUN echo "memory_limit=-1" > $PHP_INI_DIR/conf.d/memory-limit.ini

# Time Zone
RUN echo "date.timezone=${PHP_TIMEZONE:-UTC}" > $PHP_INI_DIR/conf.d/date_timezone.ini

# Register the COMPOSER_HOME environment variable
ENV COMPOSER_HOME /composer

# Add global binary directory to PATH and make sure to re-export it
ENV PATH /composer/vendor/bin:$PATH

# Allow Composer to be run as root
ENV COMPOSER_ALLOW_SUPERUSER 1

# Setup the Composer installer
ENV COMPOSER_INSTALLER_SHA384 "7228c001f88bee97506740ef0888240bd8a760b046ee16db8f4095c0d8d525f2367663f22a46b48d072c816e7fe19959"
RUN php -r "readfile('https://getcomposer.org/installer');" > /tmp/composer-setup.php \
  && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== '${COMPOSER_INSTALLER_SHA384}') { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }"

# Set up the volumes and working directory
VOLUME ["/app"]
WORKDIR /app

# Set up the command arguments
CMD ["-"]
ENTRYPOINT ["composer", "--ansi"]
