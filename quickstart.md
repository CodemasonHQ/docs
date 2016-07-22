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
$ git clone _________
$ cd _________
```

Codemason relies heavily on Git. Every Codemason application is a git repository. It at the very least must be a local git repository which you can initialise with `git init` in your project directory.

Let's make a change to `views/index.php` so you can leave your mark on this application
```html
...
<div class="container">
    <div class="content">
        <div class="title">~ Your new message ~</div>
    </div>
</div>
...
```

Commit your changes to source control (Git)
```
$ git add .
$ git commit -m "Creating my first Codemason app"
```
You now have a functioning git repository and have committed your own changes to it. Let's have a play with it!

<a name="development-environment"></a>
## Development environment
Codemason leverages the power of Docker throughout an application's lifecycle. This way, you and your team can be confident that your code will run right every time.

> You don't need to be a Docker expert to use Codemason.

With the Mason CLI, you can Dockerize your apps with a single command which generates the Docker files required and adds them to the current working directory
```
$ mason craft --with="php"
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
$ mason create

   Creating application on Codemason...

     Application name (spoon-guide-1451)
     Application path (/Users/ben/pebble)
               Domain (spoon-guide-1451.mason.ci)

   ✔ Created application
   ✔ Created remote repository
   ✔ Added git remote codemason
```

This command creates an application on Codemason for you, prepares a `git remote` repository to transport your code and updates the `.gitlab-ci.yml` so Codemason knows how to build your app.


Commit the changes to your build script:
```
$ git add .
$ git commit -m "Build script"
```

You can now deploy your app:
```
$ mason deploy

   Deploying application to Codemason...

      Uploading [====================] 100% 0.0s
       Building [====================] 100% 0.0s, ✔ passed
      Launching [====================] 100% 0.0s

     *´¨)
    ¸.•´ ¸.•*´¨) ¸.•*¨)
   (¸.•´ (¸.•` ¤ Application deployed and running at hello-world-1234.mason.ci
```

The `deploy` command takes the code you've checked in to `git` and sends it to your Codemason `git remote`. It then builds your Docker image and pushes it to the private registry. Then, Codemason spins up your app on your server.

<a name="updating-your-app"></a>
## Updating your app
Updating your app is just as easy. 

Simply modify your application as you would normally.

Then add the modified files to git
```
$ git add .
```
And commit the changes
```
$ git commit -m "Update app"
```

Deploy
```
$ mason deploy
```


## Next steps
[todo]