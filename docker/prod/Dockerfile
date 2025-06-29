# This is a multi-stage Dockerfile for production.
# It builds the Angular app and then copies the static assets
# into a final FrankenPHP image that serves both the API and the frontend.

# Stage 0: Get Composer binary
FROM composer:2.7 AS composer

# Stage 1: Build the Angular frontend
FROM node:20-alpine AS frontend-builder

WORKDIR /app/frontend

# Copy only package files to leverage Docker cache
COPY frontend/package*.json ./
RUN npm install

# Copy the rest of the frontend source code
COPY frontend/ .

# Build the Angular app for production.
# The output will be in /app/frontend/dist/frontend/browser/
# Note: The output path 'dist/frontend/browser' depends on your 'angular.json' configuration.
# Adjust if your project name is not 'frontend'.
RUN npm run build -- --configuration production


# Stage 2: Build the final production image with FrankenPHP
FROM dunglas/frankenphp:1.1-php8.3 AS final-build

WORKDIR /app

# Install required PHP extensions (e.g., for PostgreSQL)
RUN install-php-extensions pdo_pgsql intl zip ctype iconv tokenizer xml

# Install git for Composer source installs
RUN apk add --no-cache git

# Copy composer files from the backend directory
COPY backend/composer.* ./

# Copy the Composer binary from the composer stage before running it
COPY --from=composer /usr/bin/composer /usr/local/bin/composer

# Install composer dependencies for production (optimizations included)
RUN set -eux; \
	composer install --prefer-dist --no-progress --no-scripts --no-dev -o; \
	composer clear-cache

# Copy the backend application code
COPY backend/ .

# Copy the built frontend assets from the builder stage into the public web root
COPY --from=frontend-builder /app/frontend/dist/frontend/browser/ /app/public/

# Set permissions for the cache and logs directories for Symfony
RUN chown -R www-data:www-data var

# Expose HTTP, HTTPS, and HTTP/3 ports
EXPOSE 80 443 443/udp

# Set the default command to run FrankenPHP
CMD ["frankenphp", "run"]