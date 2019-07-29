# Deploying and Hosting WordPress (PHP)

- [Introduction](#introduction)
- [Creating a Wordpress application](#creating-a-wordpress-application)
- [Increase Wordpress maximum upload file size](#increase-wordpress-maximum-upload-file-size)
- [Upgrading Wordpress version](#upgrading-wordpress-version)


<a name="introduction"></a>
## Introduction
Let’s get you started with WordPress on Codemason. 

> {tip} If you plan to develop on your Wordpress instance, we highly recommend [Bedrock](https://roots.io/bedrock/). It's the same great Wordpress you know and love, just with better structure for easy deployment.

This guide will walk you through how you can successfully deploy and host Wordpress with Codemason.

> This guide assumes that you:
> - Are familiar with PHP and Wordpress
> - Have a free Codemason account 
> - Have installed the Mason CLI and its dependencies 
> - Have installed PHP and Composer locally 


<a name="creating-a-wordpress-application"></a>
## Creating a Wordpress application

Use the `mason create` command to to create a new app.
```bash
$ mason create hello-wordpress --no-remote
``` 

You'll see how we've passed a `--no-remote` flag to the `mason create` command. This is to prevent the Mason CLI from 
adding a `git remote` to the current directory since we don't need it.
 

### Create a database 

Our Wordpress instance will need a database. Create a new database service with the `mason services:create` command.

```bash
$ mason services:create hello-wordpress/db --image mariadb -p 3306:3306 \
    --env MYSQL_DATABASE=wordpress \
    --env MYSQL_USER=wordpress \
    --env MYSQL_PASSWORD=secretpass \
    --env MYSQL_ROOT_PASSWORD=secretrootpass \
	--volume /home/data/mysql/data:/var/lib/mysql
    
Creating service on Codemason...... done

    NAME     IMAGE       COMMAND     PORTS
    db       mariadb                 3306:3306
```


### Create wordpress 

Now let's spin up our new wordpress instance! 

```bash
$ mason services:create hello-wordpress/wordpress --image wordpress:5.2-apache -p 80:80 \
    --env WORDPRESS_DB_HOST=db \
    --env WORDPRESS_DB_NAME=wordpress \
    --env WORDPRESS_DB_USER=wordpress \
    --env WORDPRESS_DB_PASSWORD=secretpass \
	--volume /home/data/wordpress/wp-content:/var/www/html/wp-content
    
Creating service on Codemason...... done

    NAME          IMAGE         COMMAND     PORTS
    wordpress     wordpress                 80:80
```

### Volume permissions

After you've launched your Wordpress service, you'll want to make sure ownership permissions are set correctly so 
that Wordpress can update and/or install plugins without requiring FTP credentials. 

You can do this by running the following command:

```bash
$ mason run --service hello-wordpress/wordpress chown -R www-data:www-data /var/www/ 
```


<a name="increase-wordpress-maximum-upload-file-size"></a>
## Increase Wordpress maximum upload file size

When you're uploading a file and you see an error that says "exceeds the maximum upload size for this site",  
it's because the default Wordpress file upload limit is too small and your file is too big.

The good news is it's a very simple error to fix!

We just need to modify a few server settings. The settings that need to be changed when you're looking at how to 
increase the maximum file upload size on wordpress are:

- upload_max_filesize
- post_max_size

Since Wordpress is running in a Docker container, what we're going to do is create a new `php.ini` file on our host 
and mount it as a volume on the Wordpress service 

Create the following `php.ini` file on your server
```ini
upload_max_filesize = 50M
post_max_size = 50M
```

Here you can see we're setting the we're setting the `upload_max_filesize` and `post_max_size` to `50M` (50 MB).

Next, we'll tell Wordpress about this config file. To do this, we will mount it as a volume on the Wordpress service.

```bash
$ mason services:upgrade hello-wordpress/wordpress \
	--volume /home/data/wordpress/wp-content:/var/www/html/wp-content \
	--volume /home/data/wordpress/config/php.ini:/usr/local/etc/php/php.ini
```


<a name="upgrading-wordpress-version"></a>
## Upgrading Wordpress version

When Wordpress releases a new version, the upgrading process is very easy! Simply upgrade your service to use the most recent Wordpress version.

> {tip} A list of available Wordpress versions can be found [here](https://hub.docker.com/_/wordpress/). For simplicity, we recommend the `apache` images.

The upgrade command would look something like this:
```bash
$ mason services:upgrade hello-wordpress/wordpress \
	--image wordpress:5.3-apache
```

Where `wordpress:5.3-apache` is swapped with the Wordpress version you wish to upgrade to.

