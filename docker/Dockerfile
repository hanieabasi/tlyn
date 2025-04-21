FROM php:8.3-fpm

# Install Supervisor
RUN apt-get update && apt-get install -y \
    supervisor\
    libzip-dev\
    libpng-dev\
    libonig-dev\
    libxml2-dev \
    libcurl4-openssl-dev\
    libfreetype6-dev\
    libssl-dev\
    default-mysql-client \
    git \
    curl \
    zip \
    unzip \
    libxrender1 \
    libfontconfig1


# Install necessary PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl bcmath xml dom curl gd intl

# Install redis
RUN pecl install redis && docker-php-ext-enable redis

# Copy composer.lock and composer.json to the working directory
COPY tlyn /var/www/

# Set the working directory
WORKDIR /var/www

# Use Composer to install the application's PHP dependencies
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer install --no-interaction

RUN chown -R www-data:www-data /var/www/storage
RUN chmod -R 775 /var/www/storage

# Run composer install and database migration at entrypoint
#COPY /docker/entrypoint.sh /usr/local/bin/entrypoint.sh
#RUN chmod +x /usr/local/bin/entrypoint.sh
#ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Expose port 9000 and start Supervisor
EXPOSE 9000
CMD ["/usr/bin/supervisord"]
