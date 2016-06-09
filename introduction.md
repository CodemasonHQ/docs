# Welcome to Codemason
Every developer knows the frustration all too well — writing code only to have your flow broken by needing to configure your environment, server or deployment process.

Then before you know it, you're 5 tabs deep into how you can solve your problem in 10 different ways.

Of course, we'll repeat this process a handful of times while we stitch together a bunch of the DevOps stuff that we **must have** because otherwise our project isn't a *real* project...

Then, once we've successfully thrown our adhoc DevOps process together, we can slowly back away from our house of cards. Only to do it all again for the next project.

Codemason was passion project borne out of that frustration.

# What is Codemason?
Codemason helps developers build, deploy and manage their application.

We walk a fine line between Platform as a Service and Containers as a Service.

You could probably call us *"DevOps as a Service"*. We handle or simplify DevOps related stuff that gets in the way of developers actually developing without crippling you if you outgrow us!

And that's what really sets Codemason apart:

We don't believe in black magic and we don't like sneaky lock-in tactics. We want to operate as close to an open source project as possible.

Behind the scenes, Codemason brings together a bunch of different services and makes them talk to each other so they there's a clearly defined flow from prototype to production.

- Your Server - Codemason supports any Linux host with Docker installed. You retain total control of your server.
- Docker - we're big believer's in containerisation. Everything that Codemason runs on your server is a container. Because its containerised, we don't care what it is - we just run it.
- GitLab Community Edition
    - GitLab provides some great functionality for Codemason to integrate with that it just made sense to use them. Here's how we use GitLab.
    - Repositories - we use a private [git remote](https://git-scm.com/docs/git-remote) to seamlessly sync your repo to Codemason regardless of your git provider so deployment is as easy as `$ mason deploy`.
    - Continuous Integration - GitLab already has CI built in. Naturally we use this to enable the CI component of Codemason. Every time your remote is updated it runs the commands outlined in your config file.
    - Private Registry - we utilise GitLab's new private image registry functionality to build a new Docker image for your app on every push.
- Rancher - we use Rancher for orchestration (putting your containers on to your server)

We bring all of this together as a well packaged, clear and coherent service to give developers:
- CI/CD
- Difference environments (e.g. testing, staging, production)
- Load balancing
- One click provisioning of services like MySQL, Redis etc.

Pretty much straight out of the box 😎

> I think of Codemason as a needle and thread that ties together various concepts, practices and open source projects in a simple, natural and intuitive way.

# Next Steps
[todo]