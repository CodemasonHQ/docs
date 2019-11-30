# Deploying and Hosting Laravel (PHP)

- [Introduction](#introduction)
- [Creating a Laravel application](#creating-a-laravel-application)
- [Deploy to Codemason](#deploy)
- [Best practices](#best-practices)


<a name="introduction"></a>
## Introduction
Let’s get you started with Laravel on Codemason. 

> {tip} If you are new to Codemason, it's worth checking out the [Getting Started with PHP on Codemason](/docs/{{version}}/getting-started-with-php) guide first.

This guide will walk you through how you can successfully deploy and host your Laravel app with Codemason.

> This guide assumes that you:
> - Are familiar with PHP and Laravel
> - Have a free Codemason account 
> - Have installed the Mason CLI and its dependencies 
> - Have installed PHP, Laravel and Composer locally 


<a name="creating-a-laravel-application"></a>
## Creating a Laravel application

### Installing a new Laravel project

> {note} You'll need the Laravel installer on your local machine. If you don't already have it, the [Laravel Installation](https://laravel.com/docs/installation#installing-laravel) docs will help you get setup. 

Create our new Laravel project with the `laravel new` command. We'll call our new app `hello-laravel`.
```bash
$ laravel new hello-laravel
``` 

Once complete you will have a new directory called `hello-laravel` containing a fresh Laravel installation

### Initializing a Git repository

It's now time to initialize a Git repository and commit the current state:
```bash
$ git init
$ git add .
$ git commit -m "new laravel project"
```


<a name="deploy"></a>
## Deploying to Codemason

Let's deploy your app to Codemason! Before we can do that we must prepare our fresh Laravel project to be deployed. 


### Preparing your codebase

Before you can deploy your laravel app, you must first create a `.gitlab-ci.yml` and `Dockerfile` so Codemason knows 
how to build and run your app.
 
Don't worry, Codemason makes this super easy (and automatic) with the `mason craft` command!

```bash
$ mason craft laravel

Crafting laravel application with php, mariadb
... Wrote Dockerfile
... Wrote docker-compose.yml
... Wrote .gitlab-ci.yml
``` 

As you can see your Laravel project will now have the files your app needs. 

> {tip} Craft Kits are available for a number of languages and frameworks. You can even create and publish your own custom [Craft Kits](/docs/{{version}}/craft-kits)!  

Now commit your new files to source control
```bash
$ git add .
$ git commit -m "Preparing app for Codemason"
```

### Creating your app

Use the `mason create` command to create a new app 
```bash
$ mason create hello-laravel
```

### Pushing to Codemason

Next, push your changes to Codemason. We'll then automatically build it into a Docker image as per your `.gitlab-ci.yml` and `Dockerfile`.
```bash
$ git push codemason master 
``` 

### Deploying your app

You can now deploy your Laravel app by creating a new service. We'll call that service `web` and pass an `APP_KEY` as an env var as required by Laravel   

```bash
$ mason services:create hello-laravel/web -p 80:80 \
    --env APP_KEY=$(php artisan --no-ansi key:generate --show)
```

Congratulations! 🎉 Your new Laravel app is now live!


<a name="best-practices"></a>
## Best practices

The following best practices are particularly useful and important when you're wanting to move to production and 
make sure your app is scalable.

Like any app on Codemason, it will be run in a container. Containers have an ephemeral filesystem by default. 
This means unless you mount a volume, any changes to the filesystem will be discarded when the container 
is upgraded. The changes outlined below mostly focus on preparing your app for that.

### Logging destination
Laravel logs errors and messages to disk by default. It's a good idea to change this so your logs are sent to the output stream.

The quickest way is to just change this is to just set your `LOG_CHANNEL` env variable to `errorlog`.

> {tip} You may want to consider integrating error tracking software as well (e.g. [Sentry](https://sentry.io)) to you track, manage and monitor your errors.

[Learn more about Laravel Logging...](https://laravel.com/docs/logging)


### Session storage

By default, Laravel uses the filesystem for session storage and since the filesystem is ephemeral when you upgrade your app, the sessions stored on your disk will be lost and your users will be logged out.

Switching to the `database` session driver is a great solution to this and, like many things in Laravel, it is incredibly easy to do!

Create your `sessions` table migration:
```bash
$ php artisan session:table
$ php artisan migrate
```

Then set the `SESSION_DRIVER` env variable to `database`

[Learn more about Laravel Sessions...](https://laravel.com/docs/session)


### Cache

Laravel uses the filesystem for cache storage. Switch to the `database` driver to persist your cache.

Create your `cache` table migration:
```bash
$ php artisan cache:table
$ php artisan migrate
```

Then set the `CACHE_DRIVER` env variable to database

[Learn more about Laravel Cache...](https://laravel.com/docs/cache)


### Storage 

You will either need to mount a volume or use a driver such as Amazon S3 to store your files.

#### Mount a volume
 
If you don't intend to scale your app, you can simply mount a volume for your storage directory.

We will use a volume to replace the need to create a symbolic link from `public/storage` to `storage/app/public` - pretty cool!

```bash
$ mason services:upgrade hello-laravel/web \
    --volume /home/my-app/storage:/app/storage/app \
    --volume /home/my-app/storage/public:/app/public/storage
```

> {tip} If you are having issues with folder permissions try, running `chmod -R 755 /app/storage/app` from inside your service container.
 
#### S3 Driver

Switching to a cloud driver like `s3` is particularly important if you intend to scale your app across multiple servers, as each instance of your app will need to be able to access the filesystem.

The `s3` driver will persist your storage on Amazon S3,
granting each instance of your scaled app access to all the files.

Simply set the `FILESYSTEM_CLOUD` env variable to `s3` and update your `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_DEFAULT_REGION`, `AWS_BUCKET`, `AWS_URL` env variables accordingly.

[Learn more about Laravel Filesystem](https://laravel.com/docs/filesystem)


### Scheduler 

Laravel's command scheduler can be easily activated for your app by creating a new `scheduler` service.

Simply create a new service with your app's image and provide the command to start the scheduler.
 
```bash
$ mason services:create hello-laravel/scheduler \
    --env-file .env \
    --command sh -c "echo \"* * * * * cd /app && php artisan schedule:run >> /dev/stdout 2>&1\" | crontab - && crond -f -L /dev/stdout"
```

The `command` provided adds Cron entry that will call Laravel command scheduler every minute.


[Learn more about Laravel Task Scheduling](https://laravel.com/docs/scheduling)


### Passport

Laravel Passport encryption keys should be stored in your env vars so they will persist after upgrades. You can easily configure Passport to load the encryption keys from your environment variables with `php artisan vendor:publish --tag=passport-config`. 

> {note} Remember to commit the `config/passport.php` file to source control and push the change to Codemason.

With Passport configured to use env variables to load the encryption keys, simply set `PASSPORT_PRIVATE_KEY` and `PASSPORT_PUBLIC_KEY` values

```bash
$ mason services:upgrade hello-laravel/web \
    --env PASSPORT_PUBLIC_KEY="-----BEGIN PUBLIC KEY-----<private key here>-----END PUBLIC KEY-----" \
    --env PASSPORT_PUBLIC_KEY="-----BEGIN RSA PRIVATE KEY-----<public key here>-----END RSA PRIVATE KEY-----"
```

[Learn more about Laravel Passport](https://laravel.com/docs/passport)