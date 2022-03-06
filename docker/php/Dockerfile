FROM php:8-fpm
LABEL maintainer="edujudici@gmail.com"

# Current user
ARG USER
ARG UID

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    supervisor \
    sudo

# Install Node Specific Version
RUN curl -sL https://deb.nodesource.com/setup_12.x  | bash -
RUN apt-get -y install nodejs
RUN npm install

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN groupadd -g $UID $USER
RUN useradd -u $UID -ms /bin/bash -g $USER $USER

# Set working directory
WORKDIR /var/www

# Copy existing application directory permissions
COPY --chown=$USER:$USER . /var/www

USER $USER

# Install Laravel Envoy
RUN composer global require laravel/envoy --dev

ADD supervisord.conf /etc/supervisor/conf.d/project-name.conf

CMD ["/usr/bin/supervisord"]
