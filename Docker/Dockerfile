
# Use PHP 8.2 with FPM
FROM php:8.2.20-fpm

# Ensure all packages are updated to latest security patches
RUN apt-get update && apt-get upgrade -y

# Install PHP extensions & dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    nginx \
    git \
    zip \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    && docker-php-ext-install pdo_mysql mbstring zip exif pcntl bcmath gd \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy Nginx config
COPY ./docker/nginx.conf /etc/nginx/nginx.conf

# Copy Laravel app
COPY . /var/www/html
WORKDIR /var/www/html

# Set permissions
RUN chown -R www-data:www-data /var/www/html/storage
RUN chmod -R 775 /var/www/html/storage

# Expose ports
EXPOSE 8080 22

# Start PHP-FPM & Nginx
CMD service nginx start && php-fpm