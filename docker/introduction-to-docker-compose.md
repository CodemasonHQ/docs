# Introduction to Docker Compose

Docker Compose is a great way to define multiple container Docker environments. 
With Compose, you use a `YAML` file to configure multiple services for your app and start/stop them with ease.

Simply define the services that make up your app in a `docker-compose.yml` file so they can be run together in one isolated environment.


## Use cases 

Below are some of the main use cases of Docker Compose

### Development environment

The Compose file (`docker-compose.yml`) makes it easy to define, document and configure all of the services (e.g. databases, queues, caches, web servers etc) that your app needs.

You can launch your entire development environment with one command 
```bash
$ docker-compose up 
``` 

### Automated testing environments 

Docker Compose can fit nicely into a Continuous Integration/Deployment (CI/CD) workflow. 

By defining your app's full environment in a Compose file, in just a few commands you can create and destroy these environments:
```bash
$ docker-compose up -d
$ ./run_tests
$ docker-compose down
```

### Single host deployments

While Docker Compose isn't really intended to be used as a deployment mechanism, you can make it work with a little bit of elbow grease.
 

## Quick start

### Docker Compose file

The `docker-compose.yml` file is a YAML formatted file. For more information about the Compose file, see the [Compose file reference](https://docs.docker.com/compose/compose-file/)

Example 
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
      - "3308:3306"
    environment: 
      MYSQL_DATABASE: homestead
      MYSQL_USER: homestead
      MYSQL_PASSWORD: secret
      MYSQL_ROOT_PASSWORD: root
```

### Starting and stopping 

Start Docker Compose (and spin up all your services) with `docker-compose up`. You can stop with with `CTRL+C` (once for graceful shutdown, twice to force kill).

You can also run Docker Compose in detached mode with the `-d` flag: `docker-compose up -d`. 

When running in detected mode, you'll need to use `docker-compose stop` to shutdown your environment.

You can also start a single service in your environment `docker-compose up service-name`