# What is Docker?

Docker is a way of packaging software and all it's dependencies into a single runnable environment called a *container*.
A container has everything an application needs to run e.g. libraries, tools and code.

If you are familiar with Virtual Machines (VMs), you will see a few conceptual similarities between containers and VMs.
The key difference is containers use the low-level mechanics of the host. They do not have the high overhead and enable more efficient usage of the underlying system and resources.

There's handful of terms you'll need to be familiar with. We'll go through them now

## What is a Dockerfile?

A Dockerfile contains "instructions" for how to package up software and any dependencies it might need so it can be built into an image.

```dockerfile
FROM ubuntu 

```
## What is an image?

A Docker image is what gets built from your Dockerfile. It contains everything it needs to run and can be run anywhere Docker is installed.

## What is a container?

A container is merely an instance of the image. You can create as many containers from a single image as you want. 

Put simply: images become containers when they are run. 


## Next steps
Now you understand what docker is, we'll show you [why you should use Docker](/docker/why-use-docker) 