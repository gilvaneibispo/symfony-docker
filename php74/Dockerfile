FROM php:7.4-apache

# Instala as extensões PHP das quais o Symfony depende e/ou precisamos.
RUN apt update \
    && apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip libxml2-dev \
    && docker-php-ext-install intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && docker-php-ext-install xml

WORKDIR /var/www/my_app

# Instala o composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Instala o CLI do Symfony
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

# Configurando o Git
RUN git config --global user.email "gilvanei.pereira@gmail.com" \
    && git config --global user.name "Gilvanei Bispo"