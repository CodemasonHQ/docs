# Application architecture

- [Introduction](#introduction)
- [What are Environments](#environments)
- [What is an Application](#applications)
- [What is a Service](#services)


<a name="introduction"></a>
## Introduction
There's too much duct tape and chewing gum keeping our applications online... Codemason clears all that away. It's the same as choosing to use a framework to help us write better, cleaner, code faster. This is a "framework" to deploy and manage your apps in more reliable, less time consuming way.

Applications on Codemason are normal Docker applications, we just abstract away a lot of the tedious work that goes into running and managing them. Our focus has been to make sure that anyone can use Codemason even without experience using Docker before. 

<a name="environments"></a>
## What are Environments? 
An environment is a logical way to group certain things together and keep other things separate. Typically environments are used to enforce separation between development, staging and production. Inside an environment you can create, run and manage your applications. 

All servers, applications and services are belong to their environment. Multiple servers can be added to the same environment to form a cluster. This allows you to pool the resources of multiple servers to create a setup that's more resistant to failure. Once you're running a cluster, Codemason will orchestrate what gets run where for you automatically. 

<a name="applications"></a>
## What is an Application? 
An application is a collection of services. Those services could could be things like, a MySQL database instance and a website crafted with your favourite language.

Deploying apps to the cloud is harder than it needs to be. As developers, we should not be wasting time solving the same problems again and again or spending hours sifting through 15 different solutions to a simple question. Instead, we should be focusing on what counts - building something awesome!

That's why Codemason provides a handy catalog. The catalog simplifies the process of spinning up new services. Most of the time it's just one click or a few simple steps which we walk you through as you do it. 

<a name="services"></a>
## What is a Service? 
Services are the building blocks of your application. A service is one or more containers created from the same Docker image. This is where the magic happens, this is what makes Codemason so powerful! At the end of the day, Codemason doesn’t mind what you’re running because they’re all Docker images. You could spin up a MySQL database, a RabbitMQ message queue or your app written in PHP, Node.js, Ruby, Java, Python etc. Codemason doesn’t mind.
