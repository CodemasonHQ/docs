# Getting started with PHP on Codemason

- [Introduction](#introduction)
- [Setup](#setup)
- [Development environment](#development-environment)
- [Deploy your app](#deploy)
- [Updating your app](#updating-your-app)

<a name="introduction"></a>
## Introduction
Let’s get you started with PHP on Codemason. 

This guide will walk you through a successful deployment of a PHP application with Codemason, from prototype to production.

> This guide assumes that you already have:
> - A free Codemason account 
> - Installed the Mason CLI and its dependencies 
> - Installed PHP and Composer locally 

<a name="setup"></a>
## Setup
Start by pulling a copy of our demo PHP project. 
```
$ git clone https://github.com/CodemasonHQ/getting-started-php
$ cd getting-started-php
```

This is just a simple quote rotator app. Every time you load the page, it will randomly select a quote from an array and display it.

You will also need to install dependencies with Composer
```
$ composer install
```

<a name="development-environment"></a>
## Development environment
Codemason leverages the power of Docker throughout an application's lifecycle. This way, you and your team can be confident that your code will run right every time.

> You don't need to be a Docker expert to use Codemason.

With the Mason CLI, you can Dockerize your apps with a single command which generates the Docker files required and adds them to the current working directory
```
$ mason craft laravel

Crafting laravel application with php, mysql
... Wrote Dockerfile
... Wrote docker-compose.yml
... Wrote .gitlab-ci.yml
```

Commit our new Docker files to source control
```
$ git add .
$ git commit -m "Docker"
```

Now we've Dockerized our application, it's time to spin up our development environment (add the `-d` flag to the command to run in detached mode)
```
$ docker-compose up
```

Your application will now be running at `http://<docker-ip>`

> We recommend using [Kitematic](https://kitematic.com/), as it provides an intuitive user interface for running Docker containers.

<a name="deploy"></a>
## Deploy your app
Developer experience is our top priority. Everything we do is about solving the problems that interrupt your flow and distract you from focusing on what counts, building great apps.

First, use the `create` command to create a Codemason application. The CLI suggest default values, but you can override them as required.
```
$ mason create getting-started-php

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote codemason
```

This command creates an application on Codemason for you and prepares a `git remote` repository to transport your code.

Push your changes to your Codemason Git remote. Our GitLab CI Runner will then build your Docker image and push it to the private registry, as per your build instructions in `.gitlab-ci.yml`.
```
$ git push codemason master
```

You can now deploy your app:
```
$ mason services:create getting-started-php/web -p 80:80 --env-file .env

Creating service on Codemason...... done

    NAME     IMAGE                                        COMMAND     PORTS
    web      registry.mason.ci/benm/getting-started-php               80:80
```

And now your database:
```
mason services:create pebble/db --image mariadb -p 3306:3306 \
	--env MYSQL_DATABASE=pebble \
	--env MYSQL_USER=demo \
	--env MYSQL_PASSWORD=secret \
	--env MYSQL_ROOT_PASSWORD=supersecret \
	--volume /home/data/mysql/data:/var/lib/mysql
	
Creating service on Codemason...... done

	    NAME     IMAGE             COMMAND     PORTS
        db       mariadb                       3306:3306
```

These commands posts [Mason JSON](/docs/{{version}}/mason-json) to our [API](/docs/{{version}}/api) which spins up your service on your server for you.

<a name="updating-your-app"></a>
## Updating your app
Updating your app is just as easy the `upgrade` command.

First, let's make a change to `templates/index.phtml` so you can leave your mark on this application.
```html
...
  <div class="panel">
      <h1><i><?php echo $quote; ?></i></h1>
      <small>
        <a href="https://twitter.com/intent/tweet?text=<?php echo strip_tags($quote); ?> @codemasonhq">Tweet This!</a>
      </small>
  </div>
...
```


Add your changes to source control (Git)
```
$ git add .
```
Commit and push the changes
```
$ git commit -m "Update app"
$ git push codemason master
```

Run the upgrade command. Be sure to specify the service you wish to upgrade in the following format `application/service`.
```
$ mason services:upgrade getting-started-php/web 

Upgrading service on Codemason... Done
```
