# Use the official PHP image as the base image
FROM php:7.4-fpm

# Install additional packages and dependencies
RUN apt-get update \
    && apt-get install -y \
        libzip-dev \
        zip \
        unzip \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libssl-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) pdo_mysql zip gd

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

# Copy application code to the container
COPY . /var/www/html

# Set working directory
WORKDIR /var/www/html

# Expose the port PHP-FPM will be running on
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
