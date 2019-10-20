# PHP - Advantages of Docker


Docker has become wildly popular in recent years and for many, is a must have tool. Let's take a look at the advantages of using Docker for your PHP applications. 

The easiest way to explain Docker containers is lightweight, modular virtual machines. You can create, deploy, copy and move them from environment to environment with ease. 

Here are the advantages you can expect by using Docker with your PHP apps: 

* Ready to scale architecture 
* Automated testing, delivery and deployment processes 
* Eliminate differences between development, testing and production environments. 
* Share and distribute pre-built versions of your app 
* Easily deploy your app to production 


### Dependencies included
 
When creating a Docker image of your app, you package up all of its required dependencies and code it needs to run. Once your image is built, you will be able to run your PHP app quickly and reliably anywhere that supports Docker. 

This is a huge win if you want to be able to easily deploy your app without having to spend hours digging through what dependencies you might have forgotten to install in your new environment.

```
$ docker build . -t yellow-world
Step 1/5 : FROM codemasonhq/php:base
 ---> a60e61ea3905
Step 2/5 : COPY composer.json /app
 ---> adbb189977ed
Step 3/5 : COPY . /app
```

### Running other services 
Here's where Docker really shines. Say you want to use a MySQL database with your PHP app? Too easy. 

Using a Docker registry such as the default Docker Hub, you can access pre-built images that can also be quickly and reliably run anywhere. 

All you need to do is run the MySQL image and you'll have your new MySQL database running in a container container 
```
$ docker run --name yello-world-web -p 80:80 yellow-world
```



Then, so your app can access the database launch your app with a `--link` to it 
```
$ docker run -d --name mysql -e MYSQL_DATABASE=my-database MYSQL_ROOT_PASSWORD=secret mysql

$ docker run -d -p 80:80 --name yello-world-web --link mysql yellow-world
```

You can use the same approach to launch practically any additional service your app might need 

### Dev/prod parity  
If you've ever deployed or shared your code with someone else you'll have almost certainly be familiar with the phrase "it works on my machine!"  

Dev/prod parity is about minimising the differences between your development environment and production environment. This will protect you from having to track down bugs that only appear in production because of a slight difference in configuration.  

### Ready to scale 

Another advantage of using Docker with your PHP apps is your app will be ready to scale! Due to the way Docker works, you're naturally pushed towards a more horizontally scalable architecture. Allowing you to distribute traffic for your app across multiple different machines.

By ensuring the following, you will be able to easily scale your app PHP horizontally:

* You have no public ports set as this would result in multiple containers attempting to use the same port.
* If your service requires files to be stored, you utilise a storage service like S3 (recommended) or you have set up shared volumes.   
* Your service is stateless - the state of your system is defined by your database and shared storage, not by the individual running container instance.

### Easy to deploy

The great part about using Docker in this way is that it makes deployment very straight forward. Your Docker image contains everything
your PHP app needs, so all that you need to do is pull the image and run it on your server.
