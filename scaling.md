# Scaling

- [Introduction](#introduction)
- [Preparing to scale](#preparing-to-scale)
- [Creating a Load Balancer](#creating-a-loadbalancer)
    - [Listening ports](#listening-ports)
    - [Targets](#targets)
- [Scaling](#scaling)


<a name="introduction"></a>
## Introduction
When it comes to scaling there are two ways to do it: horizontal and vertical. Vertical scaling involves simply increasing the capacity of your server to cope with the load. Horizontal scaling is the process of adding more servers and distributing the load across them.

Codemason handles horizontal scaling with ease — when you add multiple servers to a team, they form a cluster. When you add a Load Balancer to that cluster, you can scale your applications and make them resilient to failure.

This document will step you through setting your application up so it can be scaled successfully. 


<a name="preparing-to-scale"></a>
## Preparing to scale
When you scale a service it sets the number of containers to be the value you specify. These containers run in parallel without any links between them (they are unaware of what is going on in any of the other containers).

> Remember:
> - A container is an instance of your image. 
> - A service is the collective for one or more containers running an image.

In order to successfully scale your service, ensure the following:
- You have no public ports set as this would result in multiple containers attempting to use the same port.
- If your service requires files to be stored, you utilise a storage service like S3 (recommended) or you have set up shared volumes. This prevents conflict issues when there are two containers  
- Your service is stateless - the state of your system is defined by your database and shared storage, not by the individual running container instance.


<a name="creating-a-loadbalancer"></a>
## Creating a Load Balancer
Creating a Load Balancer through the Codemason Web UI is a great experience. Simply go to the catalog, find the Load Balancer item, click "Add to Application" and launch it. 

The catalog will then step you through creating your Load Balancer. It's suggested you only use one Load Balancer per team unless there is a specific need for multiple.

<a name="listening-ports"></a>
### Listening ports
Listening ports specify which ports should accept incoming traffic. You need to be listening on that port for the Load Balancer to be able to receive and direct requests.

For standard use, setting the `Source IP/Port` to `80` will be sufficient.


<a name="targets"></a>
### Targets
Targets are the rules that tell your Load Balancer where traffic should be directed. The Codemason Web UI makes what can often be a confusing process incredibly simple and straightforward.

Let's look at setting up a couple of targets.

> An `A` record for `test` that points to the host has already been added to the `mason.ci` DNS records.

**Show my `pebble` app when someone goes to `test.mason.ci`.**

When `hostname` matches `test.mason.ci`

Target the `web` service in the `pebble` application running on port `80` in the container.


**Show the `forum` service when someone goes to `test.mason.ci/forum`**

When `hostname` matches `test.mason.ci`

And `path` matches `/forum`

Target the `forum` service in the `pebble` application running on port `80` in the container.


<a name="targets"></a>
## Scaling
Scaling your service is as simple as going to the application in the Codemason Web UI, clicking the "More" menu and clicking "Scale". Then simply enter the number of instance you wish to scale your service to.

Your Load Balancer will distribute traffic across these containers.

