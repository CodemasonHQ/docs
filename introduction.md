
# Introduction

- [Welcome to Codemason](#welcome-to-codemason)
- [What is Codemason?](#what-is-codemason)
- [What powers Codemason?](#what-powers-codemason)
- [Why Codemason?](#why-codemason)
- [Next Steps](#next-steps)

<a name="welcome-to-codemason"></a>
## Welcome to Codemason
Every developer knows the frustration all too well — writing code only to have your flow broken by needing to configure your environment, server or deployment process.

Then before you know it, you're 5 tabs deep into how you can solve your problem in 10 different ways.

Of course, we'll repeat this process a handful of times while we stitch together a bunch of the DevOps stuff that we **must have** because otherwise our project isn't a *real* project...

Then, once we've successfully thrown our adhoc DevOps process together, we can slowly back away from our house of cards and get back to coding. Only to do it all again for the next project.

Codemason was a passion project borne out of that frustration.

<a name="what-is-codemason"></a>
## What is Codemason?
Codemason helps developers build, deploy and manage their applications.

We walk a fine line between *Platform as a Service* and *Containers as a Service*.

You could probably call us *"DevOps as a Service"*. We handle or simplify DevOps related stuff that gets in the way of developers actually developing, without crippling you when you grow!

And that's what really sets Codemason apart:

We don't believe in black magic and we don't like sneaky lock-in tactics. We want to operate as close to an open source project as possible.

<a name="what-powers-codemason"></a>
## What powers Codemason?
Behind the scenes, Codemason brings together a bunch of different services and components by making them talk to each other so there's a clearly defined flow from prototype to production.

- Your Server - Codemason supports any Linux host with Docker installed. You retain total control of your server.
- Docker - we're big believers in containerisation. Everything that Codemason runs on your server is a container. Because it's containerised, we don't care what it is - we just run it.
- GitLab Community Edition
    - GitLab provides some great functionality for Codemason to integrate with so it just makes sense to use them. Here's how we use GitLab.
    - Repositories - we use a private [git remote](https://git-scm.com/docs/git-remote) to seamlessly sync your repo to Codemason, regardless of your git provider. Making deployment easy as `$ mason deploy`.
    - Continuous Integration - GitLab already has CI built in. We use this to enable the CI component of Codemason. Every time your remote is updated it runs the commands defined in your `.gitlab-ci.yml` file.
    - Private Registry - we utilise GitLab's new private image registry functionality to build a new Docker image for your app on every push.
- Rancher - we use Rancher for orchestration (putting your containers on to your server)

<a name="why-codemason"></a>
## Why Codemason?
We bring all of this together as a well packaged, clear and coherent service to give developers:
- CI/CD
- Different application environments e.g. development, staging, production
- Load balancing
- One click provisioning of services like MySQL, Redis etc.
- Smooth transitions from development to live

Pretty much straight out of the box 😎

> I think of Codemason as a needle and thread that ties together various concepts, practices and open source projects in a simple, natural and intuitive way.

<a name="next-steps"></a>
## Next Steps
Now you know a bit more about Codemason, head on over to our Five-minute quick start where we can start hacking on our first Codemason app together.