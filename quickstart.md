# Quickstart

- [Introduction](#introduction)
- [Setup](#setup)
- [Build something amazing](#build-something-amazing)
- [Development environment](#development-environment)
- [Deploy dreams](#deploy-dreams)
- [Updating your app](#updating-your-app)

<a name="introduction"></a>
## Introduction
Let's get you started with Codemason.

This quick start guide will walk you through a successful development flow with Codemason - from local development to deployment, prototype to production.

By the end of this guide, you will have
- Dockerized an application
- Spun up your development environment locally
- Created and deployed a Codemason application
- Updated your app and redeployed

All in under 5 minutes!

> This guide assumes you have:
> - A free [Codemason account](http://mason.ci/register)
> - Installed [NodeJS](https://nodejs.org/en/download/), [Git](https://git-scm.com/downloads), [Docker](https://docs.docker.com/engine/installation/) locally

<a name="setup"></a>
## Setup
Before we can get started, you will need to install the Mason CLI. The Mason CLI is distributed through NPM so installation is easy
```bash
$ npm install codemason --global
```

Once you've installed the CLI, you will be able to login to your Codemason account
```bash
$ mason login

Login to your Codemason account
Email: git@bmagg.com
Password: ********
Logged in as git@bmagg.com
```

<a name="build-something-amazing"></a>
## Build something amazing
Now it's time to write some code. For simplicity, we've prepared a simple little starter application for you which you can just clone to get started.
```bash
$ git clone https://github.com/CodemasonHQ/getting-started-php quickstart
$ cd quickstart
```

Codemason relies heavily on Git. Every Codemason application is a git repository. It at the very least must be a local git repository which you can initialise with `git init` in your project directory.

You now have a functioning git repository. Let's have a play with it!

<a name="development-environment"></a>
## Development environment
Codemason leverages the power of Docker throughout an application's lifecycle. This way, you and your team can be confident that your code will run right every time.

> {note} You don't need to be a Docker expert to use Codemason. Craft Kits will provide all the defaults you need to get started! 

With the Mason CLI, you can Dockerize your apps with a single command which generates the Docker files required and adds them to the current working directory
```bash
$ mason craft laravel

Crafting laravel application with php, mysql
... Wrote Dockerfile
... Wrote docker-compose.yml
... Wrote .gitlab-ci.yml
```

Commit our new Docker files to source control
```bash
$ git add .
$ git commit -m "Docker"
```

Now we've Dockerized our application, it's time to spin up our development environment (add the `-d` flag to the command to run in detached mode)
```bash
$ docker-compose up
```

Your application will now be running at `http://<docker-ip>`

> {tip} We recommend using [Kitematic](https://kitematic.com/) if you're unfamiliar with Docker, as it provides an intuitive user interface for running Docker containers.

<a name="deploy-dreams"></a>
## Deploy dreams
Developer experience is our top priority. Everything we do is about solving the problems that interrupt your flow and distract you from focusing on what counts, building great apps.

First, use the `create` command to create a Codemason application. The CLI suggest default values, but you can override them as required.
```bash
$ mason create quickstart

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote codemason
```

This command creates an application on Codemason for you and prepares a `git remote` repository to transport your code.

Push your changes to your Codemason Git remote. Our GitLab CI Runner will then build your Docker image and push it to the private registry, as per your build instructions in `.gitlab-ci.yml`. 
```bash
$ git push codemason master
```

You can now deploy your app:
```bash
$ mason services:create getting-started-php/web -p 80:80 --env-file .env

Creating service on Codemason...... done

    NAME     IMAGE                               COMMAND     PORTS
    web      registry.mason.ci/benm/quickstart               80:80
```

And now your database:
```bash
$ mason services:create pebble/db --image mariadb -p 3306:3306 \
	--env MYSQL_DATABASE=pebble \
	--env MYSQL_USER=demo \
	--env MYSQL_PASSWORD=secret \
	--env MYSQL_ROOT_PASSWORD=supersecret \
	--volume /home/data/mysql/data:/var/lib/mysql
	
Creating service on Codemason...... done

	    NAME     IMAGE                    COMMAND     PORTS
        db       mariadb                              3306:3306
```

These commands posts [Mason JSON](/docs/{{version}}/mason-json) to our [API](/docs/{{version}}/api) which spins up your service on your server for you.

<a name="updating-your-app"></a>
## Updating your app
Updating your app is just as easy the `upgrade` command.

Simply modify your application as you would normally.

Then add the modified files to git
```bash
$ git add .
```
Commit and push the changes
```bash
$ git commit -m "Update app"
$ git push codemason master
```

Run the upgrade command. Be sure to specify the service you wish to upgrade in the following format `application/service`.
```bash
$ mason services:upgrade quickstart/web 

Upgrading service on Codemason... Done
```
