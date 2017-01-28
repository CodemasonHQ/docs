# Preparing an app

- [Introduction](#introduction)
- [Initialize a Git repository](#initialize-git)
- [Craft your app with Docker](#craft-with-docker)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
Codemason is powered by Docker. Everything Codemason runs is a Docker service. This makes it possible to do some incredible things with anything that's already been built into a Docker image. 

The process of turning your app into a Docker image which can be run as a Docker service is somewhat tedious. It introduces new levels of complexity - base images, Dockerfiles, Automated builds, uploading it to a registry, hosting it on a registry before you can finally launch it on your server.

Instead of leaving leaving you to do that yourself, Codemason gives you an opinionated pipeline to take the code you've been writing for your app, package it up and ship it out to your server. 

To take advantage of this, you will need ensure your app is prepared correctly. This document will walk you through preparing an app to be deployed with Codemason.  


<a name="initialize-git"></a>
## Initialize a Git repository 
Codemason relies heavily on Git so before you can deploy your app through Codemason, it must be a git repository. 

You can initialise a git repository in the current folder by running:
```
$ git init
$ git add . 
$ git commit -m "Creating my first Codemason app"
```

<a name="craft-with-docker"></a>
## Craft your app with Docker
Make your app Codemason ready by Dockerizing it. You can do this manually yourself or  automatically with a craft kit. 
```
$ mason craft <craft-kit>
```

We have craft kits for many popular frameworks, architectures and languages as possible.

You can now add your Docker files to source control. 
```
$ git add .
$ git commit -m "Docker"
```

<a name="next-steps"></a>
## Next steps
After you have prepared your app, you are ready to create an app.