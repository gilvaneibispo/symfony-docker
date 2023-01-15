# Project core symfony-docker
Project core Symfony and Docker with Apache, PHP 7.4 and MySQL 8. Quick setup project!

Root user `sudo su`

### Create folder structure

```
mkdir <my_app>
cd <my_app>
mkdir app
mkdir php_74
mkdir mysql_8
```

`my_app` is application name.

### Create config file Docker Compose.

```
touch docker-compose.yml
```

### Write in file docker-compose.yml

```
version: '3.8'

services:
  php_74:
    container_name: php_74
    build:
      context: ./php_74
    ports:
      - '8080:80'
      - '9000:9000'
    volumes:
      - ./app:/var/www/<my_app>
    depends_on:
      - db_mysql8

  db_mysql8:
    container_name: db_mysql8
    image: mysql:8.0
    command: --default-authentication-plugin=mysql_native_password
    ports:
      - '3306:3306'
    volumes:
      - ./mysql_8:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: <root_pass>
      MYSQL_DATABASE: <app_db>
      MYSQL_USER: <user>
      MYSQL_PASSWORD: <pass>
```
Note: Change the values between `<` and `>` to the desired ones.

### Create PHP Dockerfile

```
touch php_74/Dockerfile
```

### Write in file Dockerfile

```
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

WORKDIR /var/www/<my_app>

# Instala o composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Instala o CLI do Symfony
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

# Configurando o Git
RUN git config --global user.email "user@mail.com" \
    && git config --global user.name "Username"
```
Note: Change Git credentials and values between `<` and `>` to the desired ones.

### Container build

```
docker-compose up -d --build
```

### PHP container terminal access.

```
docker-compose exec php_74 /bin/bash
```

### Check Symfony requirements

```
symfony check:requirements
```

Note: If everything is OK, you can create the project. Remember that now you will be using the `/var/www/my_app` folder, it has been “synced” with `app` folder.

![result](https://user-images.githubusercontent.com/21068705/212502754-cdda4e3a-231a-4c0c-b296-aa358bcc1dc4.png)

### Create Symfony project in <my_app> folder.

```
symfony new ./
```

### Start Symfony server on additional port [9000]

```
symfony server:start –port 9000 –d
```

