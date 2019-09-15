# Installing Docker on Windows

Docker Desktop for Windows is easy to install. It's the fastest and most reliable way to run Docker on Windows. 
It comes with everything you need: Docker Engine, the Docker CLI Client, Docker Compose and Docker Machine. 


## Instructions

### System Requirements 
Docker Desktop for Windows works on Windows 10 64-bit: Pro, Enterprise or Education (Build 15063 or later).

Hyper-V and Containers Windows features must be enabled.

The following hardware prerequisites are required to successfully run Client Hyper-V on Windows 10:

* 64 bit processor with Second Level Address Translation (SLAT)
* 4GB system RAM
* BIOS-level hardware virtualization support must be enabled in the BIOS settings. For more information, see Virtualization.

### Install Docker for Windows 

Download the latest stable version of Docker Desktop.

[Download Docker Desktop for Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) 

Once the download has completed, install it by double-clicking the `Docker for Windows Installer.exe` file.

Follow the instructions on the installation wizard. Be sure to accept the license and authorize the installer.

You will be prompted to authorize Docker Desktop Installer with your system password as part of the install process. This is required to install networking components, links to the Docker apps and manage Hyper-V VMs.


### Run Docker for Windows

You will need to start Docker after the installation is completed. Start Docker Desktop by opening the `Docker Desktop` app.

When Docker has started, a whale icon in the status bar will indicate if Docker Desktop is up and running and accessible. 

> {tip} The whale icon might be hidden in the Notifications area. Try clicking the up arrow on the taskbar to see if it's hidden. 

Let's open up terminal and try some Docker commands

* Run `docker --version`, `docker-compose --version` and `docker-machine --version` to check the installation
* Run `docker run hello-world` to run the `hello-world` Docker example and verify everything is working as expected


### Use Docker

Congratulations! You now have Docker installed and running on your PC.


## Next steps
Now you have Docker installed and running on your PC, let's look at [building a Docker image](/docker/building-a-docker-image).