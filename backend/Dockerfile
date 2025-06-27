# Choose the FrankenPHP image for your project
FROM dunglas/frankenphp:1.1-php8.3

# Set the working directory
WORKDIR /app

# Copy composer files first to leverage Docker's layer cache.
# This prevents re-installing vendors every time a code file changes.
COPY composer.* symfony.lock ./

RUN set -eux; \
	composer install --prefer-dist --no-progress --no-scripts; \
	composer clear-cache

# Copy the rest of the application
COPY . .

# If you need to install other PHP extensions, uncomment the following line
# and add the extension names. For example, for PostgreSQL and intl:
# RUN install-php-extensions pdo_pgsql intl