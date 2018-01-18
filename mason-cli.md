# Command Line Installation and Setup

- [Introduction](#introduction)
- [Installation](#installing-the-cli)
- [Updating the CLI](#updating-the-cli)

<a name="introduction"></a>
## Introduction
The Mason CLI (command line interface) is a helpful command line tool you can use to improve your development and deployment experience. It wraps around the Codemason API and abstracts away the tedious bits so you can get back to focusing on building your app. 

With the Mason CLI, we also introduced [Craft kits](/docs/{{version}}/craft-kits). Craft kits are the hassle free way to setup a Docker development environment for the language/framework of your choice. They are an excellent way to ease into building Docker powered apps without having to learn the ins and outs of Docker.

<a name="installing-the-cli"></a>
## Installing the CLI 
Install the Mason CLI as a global NPM package

```
$ npm install --global codemason
```

Dependencies: NodeJS, Git, Docker

<a name="updating"></a>
## Updating the CLI
We make improvements to the Mason CLI regularly so it's a good idea to stay up-to-date for the best experience! To update the Mason CLI simply run the self update command.

```
$ mason self-update
```
