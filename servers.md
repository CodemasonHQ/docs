# Adding servers

- [Overview](#overview)
- [Adding servers](#adding-servers)


<a name="introduction"></a>
## Introduction
Before you will be able to run any apps with Codemason, you will need to have added at least one server to team of your choosing. When multiple servers are added to a team, they form a cluster where their resources are pooled and services are automatically spread across to utilise resources more effectively. 

You can add any Linux server running Docker 1.10.3+. 

> We take care of your infrastructure, you retain total control. 

<a name="adding-servers"></a>
## Adding servers
Adding a server to Codemason is very simple. Once you have provisioned a Linux, server with Docker 1.10.3+ installed, go to Servers and click the "Add Server" button.

Codemason will generate a Docker run command for you. Simply run that command on your server. This will launch a rancher-agent container on your server. It will then contact Codemason and automatically register itself with your team. 

The generated command is unique to your team, so always be sure to copy the command from Codemason each time you want to register a server. 

