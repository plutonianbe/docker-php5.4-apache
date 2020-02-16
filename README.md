# PHP 5.4 Apache

PHP 5.4 [reached EOL](http://php.net/eol.php) on 3 Sep 2015 and thus, official docker support was dropped.

# What is PHP?

PHP is a server-side scripting language designed for web development, but which can also be used as a general-purpose programming language. PHP can be added to straight HTML or it can be used with a variety of templating engines and web frameworks. PHP code is usually processed by an interpreter, which is either implemented as a native module on the web-server or as a common gateway interface (CGI).

> [wikipedia.org/wiki/PHP](http://en.wikipedia.org/wiki/PHP)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/php/logo.png)

# How to use this image.

## With Command Line

For PHP projects run through the command line interface (CLI), you can do the following.

### Create a `Dockerfile` in your PHP project

    FROM plutonianbe/php54-apache:latest
    COPY . /usr/src/myapp
    WORKDIR /usr/src/myapp
    CMD [ "php", "./your-script.php" ]

Then, run the commands to build and run the Docker image:

    docker build -t my-php-app .
    docker run -it --rm --name my-running-app my-php-app

### Run a single PHP script

For many simple, single file projects, you may find it inconvenient to write a complete `Dockerfile`. In such cases, you can run a PHP script by using the PHP Docker image directly:

    docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp plutonianbe/php54-apache:latest php your-script.php

### Installing modules

To install additional modules use a `Dockerfile` like this:

``` Dockerfile
FROM plutonianbe/php54-apache:latest

# Installs curl
RUN docker-php-ext-install curl
```

Then build the image:

``` bash
$ docker build -t my-php .
```

### Without a `Dockerfile`

If you don't want to include a `Dockerfile` in your project, it is sufficient to do the following:

```bash
docker run -it --rm --name my-php-app -v "$PWD":/var/www/html plutonianbe/php54-apache:latest
```

### Use image with `docker-compose`

```yml
version: "3.1"
 services:

   app:
     image: plutonianbe/php54-apache:latest
     depends_on:
       - mysql
     environment:
       - XDEBUG_CONFIG=remote_host=${EN0IP}
     links:
       - mysql
     ports:
       - 80:80
     restart: always
     volumes:
       - ./:/var/www/html

   # Optional container if you are using mysql. Don't forget to change the credentials
   mysql:
     image: mysql:5.5
     environment:
       - MYSQL_ROOT_PASSWORD=root
       - MYSQL_DATABASE=app
       - MYSQL_USER=app
       - MYSQL_PASSWORD=app
     ports:
       - 3306:3306
     volumes:
       - ./docker/data:/var/lib/mysql
```

If you want to use xdebug don't forget to set the env variable `EN0IP`. For example add `export EN0IP=$(ipconfig getifaddr en0)` to you `.bashrc` or `.zshrc`.

# License

View [license information](http://php.net/license/) for the software contained in this image.
