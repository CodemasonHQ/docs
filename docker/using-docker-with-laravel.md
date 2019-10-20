# Using Docker with Laravel

Let's take a quick look at how you can successfully use Docker with Laravel.
By using Docker for your Laravel app, it will help speed up development and deployment.

This guide is a step by step guide that will walk you through the process of using Docker with Laravel.


## Install Docker

The first thing you will need is Docker installed on your machine.

* [Install Docker on MacOS](docker/installing-docker-on-macos)
* [Install Docker on Windows](/docker/installing-docker-on-windows)

If you're unfamiliar with Docker, you may want to [learn more about what Docker is](docker/what-is-docker) and [why you would use Docker](docker/why-use-docker). 

## Create a Laravel project

Let's get your Laravel setup. Simply run the `laravel new` command

```bash
$ laravel new my-laravel-project
```

This will create a fresh Laravel project called `my-laravel-project`.


## Write a Dockerfile

Now we're going to add a `Dockerfile` to our Laravel project. The Dockerfile contains all the steps to build an image of our Laravel app.

> {tip} We've documented everything we've learned about [building Docker images](/docker/building-a-docker-image) based on our experience building and then deploying countless apps.  

For this guide our Dockerfile will be kept short and simple. Add the following `Dockerfile` to your project directory

```dockerfile
FROM php:7

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Enable additional PHP extensions (required for Laravel) 
RUN docker-php-ext-install pdo mbstring

# Copy project files into Docker container
WORKDIR /app
COPY . /app

# Install dependencies 
RUN composer install

# Run our Laravel app
CMD php artisan serve --host=0.0.0.0 --port=80
``` 

## Docker Compose 

We're just going to jump straight into using Docker Composer to build your image and run your app. This is much more practical and convenient.

Add a `docker-compose.yml` file to your project 
```yaml
version: "3.7"
services:
  web:
    build: .
    volumes:
      - ./:/app
    ports:
      - "80:80"
    
  mysql:
    image: mariadb
    volumes:
      - ./storage/data/mysql:/var/lib/mysql
    ports:
      - "3306:3306"
    environment: 
      MYSQL_DATABASE: laravel
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_ROOT_PASSWORD: 
```

Here we're adding two services: `web` and `mysql`. 
The `web` service will be build using the `Dockerfile` we created and the `mysql` service will run the `mariadb` image. 


## Build your Docker Image 

With your `docker-compose.yml` file in place, all you need to do now is start a build
```bash
$ docker-compose build
```

This will build an image based on the steps in your `Dockerfile`.


## Run your app

Now everything is built, let's start up your development environment

```bash
$ docker-compose up 
```

## Deployment 

Once created, Docker images can be pushed to a Docker registry to be shared with other users or servers. 

Push your image to the Docker Hub registry by completing the following steps: 

1. Create a [Docker Hub](https://hub.docker.com) account 
2. Log into Docker Hub with `docker login`
3. Push the image to the registry `docker-compose push`

That's it. Now your image will be on Docker Hub so you can pull and run it from any computer with Docker installed.

```bash
$ docker run [your-username]/laravel-project
```

Of course, the image will need to be re-built and re-pulled as you make changes to your code. 
You can automate this with automated build tools/services and connect those to an automated deployment tool.


## Deployment with Codemason

[Codemason](https://codemason.io) is a tool that streamlines the entire process of deploying your Laravel app (building, pulling running Docker images) on the server of your choice.

With Codemason, all you need to do is run `git push codemason master` and your changes will be deployed to your server.

[Learn more about deploying Laravel with Codemason...](/docs/{{version}}/deploying-laravel)