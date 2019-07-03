# Setting up Local Development

- [Introduction](#introduction)
- [Craft kits](#craft-kits)
- [Launching your development environment](#launching-development)

<a name="introduction"></a>
## Introduction
Setting up a development environment is a pleasure with the Mason CLI and craft kits. 

Simply run a single command and your application will be Dockerized 
```bash
$ mason craft <craft-kit>
```

For example 
```bash
$ mason craft laravel 
```

The above command pulls the craft kit from [codemasonhq/craft-kit-laravel](https://github.com/CodemasonHQ/craft-kit-laravel) and generates a `Dockerfile` and `docker-compose.yml` file for you based on what's defined in the craft kit. The generated files will be added to the current working directory.

Or if you want to get a little bit more specific, you can specify exactly what containers from the craft kit you want to use.
```bash
$ mason craft laravel --with="php, postgres"
```

<a name="craft-kits"></a>
## Craft kits
Craft kits are a super flexible and portable way to define Docker environments. They are an excellent way to ease into building Docker powered apps without having to learn the ins and outs of Docker.

Official craft kits provide an opinionated starting point for Dockerizing your apps in a single command. 

You can find more information and all the official craft kits within the [CodemasonHQ organisation](https://github.com/CodemasonHQ?utf8=%E2%9C%93&q=craft-kit&type=&language=). Each craft kit is prefixed with `craft-kit-` to make them easy to find. 


<a name="launching-development"></a>
## Launching your development environment
Now you need to do is spin up your environment with Docker 
```bash
$ docker-compose up 
```

That's all! You're now running your Laravel application with Docker!

You'll be able to access your application at `http://<docker-ip>`, where `<docker-ip>` is the boot2docker ip or localhost if you are running Docker natively.

> We also recommend using [Kitematic](https://kitematic.com/), as it provides an intuitive user interface for running Docker containers.