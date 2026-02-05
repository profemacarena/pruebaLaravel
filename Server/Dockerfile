FROM php:7.4-apache

RUN apt-get update && apt-get install -y \
    git unzip zip libzip-dev libpq-dev libonig-dev libxml2-dev \
 && docker-php-ext-install pdo pdo_pgsql zip mbstring xml

RUN a2enmod rewrite

WORKDIR /var/www/html
COPY . .

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Dependencias
RUN composer install --no-dev --optimize-autoloader --no-interaction --no-progress --no-scripts


# Apache -> /public (si tu Laravel tiene carpeta public)
RUN sed -i 's|DocumentRoot /var/www/html|DocumentRoot /var/www/html/public|g' /etc/apache2/sites-available/000-default.conf

EXPOSE 80

CMD php artisan config:clear && php artisan cache:clear && php artisan migrate --force && apache2-foreground


