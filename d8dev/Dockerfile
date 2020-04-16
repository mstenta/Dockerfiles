# Inherit from the official Drupal 8 image.
FROM drupal:8

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer

# Install unzip (needed by Composer for unzipping packages).
RUN apt-get update -y \
  && apt-get install -y unzip

# Generate an empty project from drupal/recommended-project.
# @todo What happens if drupal/recommended-project changes to D9 in the future?
RUN composer create-project drupal/recommended-project /var/www/recommended-project

# Delete /var/www/html and copy from the empty project.
RUN rm -r /var/www/html \
  && cp -r /var/www/recommended-project /var/www/html \
  && chown -R www-data:www-data /var/www/html

# Configure Apache to use /var/www/html/web as the document root.
ENV APACHE_DOCUMENT_ROOT=/var/www/html/web
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
  && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Install phpunit.
RUN composer require --dev phpunit/phpunit

