# image
FROM php:5.6-apache

# Set php.ini
COPY ./config/php/php.ini /usr/local/etc/php/

# Set apache conf (Before tag:5.6-apache)
COPY ./config/php/apache2.conf /etc/apache2/
COPY ./config/php/sites/*.conf /etc/apache2/sites-available/

COPY ./html/composer.json /var/www/html

# Set apache conf (After tag:5.5-apache)
# ENV APACHE_DOCUMENT_ROOT /var/www/html/test
# RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
# RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Install MySQL connection module
RUN apt-get update \
  && apt-get install -y libpq-dev \
  && docker-php-ext-install pdo_mysql pdo_pgsql mysqli mbstring \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer

RUN apt-get install -y zip unzip

RUN cd /var/www/html \
  && composer install

ENV PATH $PATH:./vendor/phpunit/phpunit