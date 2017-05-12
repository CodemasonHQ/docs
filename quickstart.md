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
```
$ npm install codemason --global
```

Once you've installed the CLI, you will be able to login to your Codemason account
```
$ mason login

   Please login or create an account by entering an email and password:

          Site: https://mason.ci
         Email: git@bmagg.com
      Password: *********

   ✔ Login successful
```

<a name="build-something-amazing"></a>
## Build something amazing
Now it's time to write some code. For simplicity, we've prepared a simple little starter application for you which you can just clone to get started.
```
$ git clone https://github.com/CodemasonHQ/getting-started-php quickstart
$ cd quickstart
```

Codemason relies heavily on Git. Every Codemason application is a git repository. It at the very least must be a local git repository which you can initialise with `git init` in your project directory.

You now have a functioning git repository. Let's have a play with it!

<a name="development-environment"></a>
## Development environment
Codemason leverages the power of Docker throughout an application's lifecycle. This way, you and your team can be confident that your code will run right every time.

> You don't need to be a Docker expert to use Codemason.

With the Mason CLI, you can Dockerize your apps with a single command which generates the Docker files required and adds them to the current working directory
```
$ mason craft laravel
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

<a name="deploy-dreams"></a>
## Deploy dreams
Developer experience is our top priority. Everything we do is about solving the problems that interrupt your flow and distract you from focusing on what counts, building great apps.

First, use the `create` command to create a Codemason application. The CLI suggest default values, but you can override them as required.
```
$ mason create --application quickstart

   Creating application on Codemason...

⁣     Application name (quickstart)
⁣         Service name (web)
⁣         Service path (/Users/ben/quickstart)

   ✔ Created application
   ✔ Created remote repository
   ✔ Added git remote codemason
```

This command creates an application on Codemason for you and prepares a `git remote` repository to transport your code.

Push your changes to your Codemason Git remote. Our GitLab CI Runner will then build your Docker image and push it to the private registry, as per your build instructions in `.gitlab-ci.yml`. 
```
$ git push codemason master
```

You can now deploy your app:
```
$ mason deploy --to quickstart

   Deploying application to Codemason...

      Uploading [====================] 100% 0.0s
       Building [====================] 100% 0.0s, ✔ passed
      Launching [====================] 100% 0.0s

     *´¨)
    ¸.•´ ¸.•*´¨) ¸.•*¨)
   (¸.•´ (¸.•` ¤ Application deployed and running at hello-world-1234.mason.ci
```

The `deploy` command posts [Mason JSON](/docs/{{version}}/mason-json) to our [API](/docs/{{version}}/api) which spins up your app on your server for you.

<a name="updating-your-app"></a>
## Updating your app
Updating your app is just as easy the `upgrade` command.

Simply modify your application as you would normally.

Then add the modified files to git
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
$ mason upgrade quickstart/web
```
