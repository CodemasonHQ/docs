# Installing Docker on MacOS

Docker Desktop for Mac is easy to install. It's the fastest and most reliable way to run Docker on your Mac. 
It comes with everything you need: Docker Engine, the Docker CLI Client, Docker Compose and Docker Machine. 


## Instructions

### System Requirements 
Docker Desktop for Mac works on OS X Sierra 10.12 and newer macOS releases. 

### Install Docker for Mac 

Download the latest stable version of Docker Desktop.

[Download Docker Desktop for Mac](https://download.docker.com/mac/stable/Docker.dmg) 

Once the download has completed, install it by double-clicking the `Docker.dmg` file.

### Run Docker for Mac

When the installation is completed and Docker has started, you will see the little Docker whale in your status bar. 

Let's open up terminal and try some Docker commands

* Run `docker --version`, `docker-compose --version` and `docker-machine --version` to check the installation
* Run `docker run hello-world` to run the `hello-world` Docker example and verify everything is working as expected


### Use Docker

Congratulations! You now have Docker installed and running on your Mac.


## Next steps
Now you have Docker installed and running on your Mac, let's look at [building a Docker image](/docker/building-a-docker-image).