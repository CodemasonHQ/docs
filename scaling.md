# Scaling

- [Introduction](#introduction)
- [Preparing to scale](#preparing-to-scale)
- [The Load Balancer](#the-load-balancer)
- [Manual Scaling](#manual-scaling)
- [Autoscaling](#autoscaling)


<a name="introduction"></a>
## Introduction
When it comes to scaling there are two ways to do it: horizontal and vertical. Vertical scaling involves simply increasing the capacity of your server to cope with the load. Horizontal scaling is the process of adding more servers and distributing the load across them.

Codemason handles horizontal scaling with ease — when you add multiple servers to a team, they form a cluster. You can scale your apps across multiple servers and server providers to make them resilient to failure.

This document will step you through setting your application up so it can be scaled successfully. 


<a name="preparing-to-scale"></a>
## Preparing to scale
When you scale a service it sets the number of containers to be the value you specify. These containers run in parallel without any links between them (they are unaware of what is going on in any of the other containers).

> Remember:
> - A container is an instance of your image. 
> - A service is the collective for one or more containers running an image.

In order to successfully scale your service, ensure the following:
- You have no public ports set as this would result in multiple containers attempting to use the same port.
- If your service requires files to be stored, you utilise a storage service like S3 (recommended) or you have set up shared volumes.   
- Your service is stateless - the state of your system is defined by your database and shared storage, not by the individual running container instance.


<a name="the-load-balancer"></a>
## The Load Balancer

The key to successfully scaling is the load balaner. By default, each team on Codemason has a load balancer built into it. You can view this load balanacer by navigating to the *applications* page, clicking the dropdown menu and clicking `Load Balancer`.

Your load balancer will route traffic to your apps by choosing a corresponding container based on the rules provided. 


<a name="manual-scaling"></a>
## Manual Scaling

Scaling your service is as simple as going to the application in the Codemason Web UI, clicking the "More" menu and clicking "Scale". Then simply enter the number of instance you wish to scale your service to.

Your Load Balancer will distribute traffic across these containers.


<a name="autoscaling"></a>
## Autoscaling

Autoscaling lets you scale your app up and down based on your app's resource consumption. Autoscale keeps your apps running at peak performance during unexpected traffic spikes by scaling up with demand and reduces your running costs by scaling down during quiet periods.

Autoscale can be enabled from your app's the "Resources" by clicking the "Enable Autoscaling" button. 
You may specify an allowed autoscaling range as well as the server provider you wish to use for scaling.

> {note} Existing or additional servers added are considered separate and not counted towards the min/max range. The range limits only count servers added by autoscaling.     
 
When you are happy with your autoscale configuration, click "Save". Then once saved, Codemason will automatically autoscale your Docker containers as per your configuration.