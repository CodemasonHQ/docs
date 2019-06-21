# Getting started with PHP on Codemason

- [Introduction](#introduction)
- [Setup](#setup)
- [Prepare your app](#prepare)
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
Start by installing the [Mason CLI](/docs/{{version}}/mason-cli). You can use the CLI to manage and deploy your apps, view application logs and run your apps locally.

Install the Mason CLI
```
$ npm install --global codemason
```

With the Mason CLI installed, you will be able to use the `mason` command from your command line.

Use the `mason login` command to log in to Codemason: 
```
$ mason login
```

<a name="prepare"></a>
## Prepare your app
Let's prepare an app to deploy!

If you're new to Codemason, we recommend using our sample PHP app to complete this tutorial. However, if you want to deploy an existing app, see [this guide](/docs/{{version}}/preparing-apps) for preparing your codebase for Codemason deployment.

Start by pulling a copy of our demo PHP project. This is just a simple quote rotator app. Every time you load the page, it will randomly select a quote from an array and display it.
```
$ git clone https://github.com/CodemasonHQ/getting-started-php
$ cd getting-started-php
```

You now have a git repository that contains our sample PHP app as well as a `Dockerfile` and `.gitlab-ci.yml` file. These files tell Codemason how to build and run your PHP app. 

<a name="deploy"></a>
## Deploy your app
Now it's time to deploy our app to Codemason.

At Codemason, developer experience is our top priority. Everything we do is about solving the problems that interrupt your flow and distract you from focusing on what counts, building great apps.

First, use the `mason create` command to create a Codemason app. The CLI suggest default values, but you can override them as required.
```
$ mason create getting-started-php

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote codemason
```

This command creates an app and prepares Codemason to receive your code.

Push your changes to your Codemason Git remote. Our GitLab CI Runner will then build your Docker image and push it to the private registry, as per the build instructions in `.gitlab-ci.yml`.
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
mason services:create getting-started-php/db --image mariadb -p 3306:3306 \
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
