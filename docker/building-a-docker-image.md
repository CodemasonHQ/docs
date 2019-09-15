# Building a Docker image

We're going to teach you everything we've learned about building Docker images. 
This is based on our experience building and then deploying countless Docker apps on Codemason.


## Basic Principles 


This is not a list of hard rules that must be followed, merely suggestions that have repeatedly served us well 
while building and deploying many modern software-as-a-service apps using Docker.  

> {note} These principles are loosely based on the [12factor app](https://12factor.net) methodology, a programming language agnostic set of best practices for deployment of cloud-native applications.  

### Codebase in Git

The codebase for your app should always be checked in to a version control system such as Git.
An app should only have one codebase. If there are multiple codebase's, they can be split out into separate apps.


### Run one codebase 

A good Docker image only focuses on running one app codebase. 
That means if your image is trying to run multiple app codebase's, you should split them into different apps so they can be built as separate images.

One Docker image = one app.


### Dependencies included

Your image needs to be portable. You want to be able to run it anywhere. 

Your Docker image needs to contain all the OS dependencies, code and code dependencies your app needs. 

Use the `Dockerfile` install OS dependencies. 

Take advantage of package distribution systems in almost all modern programming languages to track code dependencies of your app.

For example, PHP uses Composer for dependency declaration, Python uses Pip, Ruby uses Bundler and Node.js uses package.json. 

### Codebase included

Your `Dockerfile` should `COPY` your codebase into the Docker image. By building your app's codebase into the image, you will be able to deploy your app anywhere.

When in development, you can simply mount your codebase as a volume and have your code running in the exact same environment.


### Store config in environment

Any config variables that change regularly should be stored as environment variables.

This will make it easy to change config without having to change any code.

A good way to test whether your image (and codebase) has the right config variables as env vars is whether the codebase could be made open source at any moment,
without compromising any credentials.
 

### Stateless

It's best for your Docker image and codebase to be stateless to avoid issues losing state when deploying, restarting, scaling etc.

This means any data that needs to be persisted should be stored in something like a database and not on the filesystem.


### Dev/prod parity 

Build your Docker image so it can be used in development and in production. The goal is to keep development and production as similar as possible.

If you introduce differences between the your development and production environments, you run the risk of introducing differences between the environments which can lead to hard to identify/resolve bugs.


### Keep it small

Keeping your image as small as possible is a good goal. You want to be able to move your image around with ease, so the smaller the better. 
Opting for `alpine` based images is usually a good way to achieve this.
  



## Your first `Dockerfile`

The `Dockerfile` is the key ingredient to creating a Docker image, it contains all the instructions required to build the Docker image.  

A simple place for your `Dockerfile` to live is in the root of your codebase. Once you've created your `Dockerfile` you'll need to choose a base image. 
Your base image can be a clean operating system or an existing image that someone else has already created.

If you want to start from a clean operating system, `alpine` is usually a good option. 

```dockerfile
FROM alpine:3.9
```

### Building your Docker image

Once you've laid out all the instructions for how your image should be built, it's time to actually run the build

```bash
$ docker build . -t your-new-image-name
Sending build context to Docker daemon
# ...
# ... Build output
# ... 
Successfully built 58e0d389dfc0
```

#### Adding the codebase

You should be copying your app's codebase into the Docker image. Do this with the `COPY` directive
```dockerfile
FROM alpine:3.9

# Add project files
COPY . /app
```

The advantage of this approach is when you're in development, you can easily use a volume to inject your development code into your container.   
In the [Building with Docker Compose]() section, we'll show you how to do this using Docker Compose. 

#### Building with Docker Compose

Running the build command is fine when you're testing things out. 
But if you're using Docker to run your development environment, you'll want to be using Docker Compose. 

You can do this seamlessly by creating a service with a `build:` configuration option
```yaml
version: "3.7"
services:
  web:
    build: .
```

Now if you need to build the Docker image for your app you can simply run 
```bash
$ docker-compose build
```

You'll also want to mount your code as a volume so all your changes make it into container automatically. 
You can do this by adding a `volume:` configuration option
```yaml
version: "3.7"
services:
  web:
    build: . 
    volumes:
      - ./:/app
```


### Debugging builds

Builds don't always go to plan and some times build logs may not always provide enough information to determine what changes you need to make.

As part of Docker's build process, every successful step in the build outputs an ID of the intermediate image that was created.
```
Step 3 : COPY package.json /app
 ---> 815a6c811367
Step 4 : RUN composer install
```  

Since the Docker build is creating intermediate images named with the ID (e.g. `815a6c811367`), we can run them and debug the issue directly with an interactive shell session
```bash
$ docker run --rm -it 815a6c811367 /bin/bash
```

Once you've detected the source of the problem, you can add the steps required to resolve it to your `Dockerfile`


## Next steps
You're building Docker images like a pro! Let's make this a little bit more practical and easy to use with an [introduction to Docker Compose](/docker/introduction-to-docker-compose). We'll have you spinning up a full development environment for your app in no time!  