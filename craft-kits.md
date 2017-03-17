# Craft Kits

- [Introduction](#introduction)
- [Custom Craft Kits](#custom)
- [Local Craft Kits](#local)
- [Creating a Craft Kit](#creating)

<a name="introduction"></a>
## Introduction
Craft kits are a super flexible and portable way to define Docker environments. They are an excellent way to ease into building Docker powered apps without having to learn the ins and outs of Docker.

Official craft kits provide an opinionated starting point for Dockerizing your apps. We've carefully selected and specifically created Docker images that play together nicely so you can Dockerize your apps with a single command.

You can find all the official craft kits within the [CodemasonHQ organisation](https://github.com/codemasonhq) with the prefix `craft-kit-`. We aim to support as many popular frameworks, architectures and languages as possible.

Officially supported craft kits: 
- [Laravel](https://github.com/codemasonhq/craft-kit-laravel) 
- [Wordpress](https://github.com/codemasonhq/craft-kit-wordpress) *(beta)*
- [Rails](https://github.com/codemasonhq/craft-kit-laravel) *(beta)*

Community contributed craft kits: 
- [PRs welcome](https://github.com/CodemasonHQ/docs/blob/master/craft-kits.md)

<a name="custom"></a>
## Custom Craft Kits
While we recommend the official craft kits for simplicity and compatibility with what we have planned, custom craft kits give you an added level of flexibility. Anyone can create a craft kit and use it with: 
```
$ mason craft username/repo
```
Where `username/repo` is the short hand for the repository to retrieve. 
- GitHub - `github:username/repo` or simply `username/repo`
- GitLab - `gitlab:username/repo`
- BitBucket - `bitbucket:username/repo`

By default it will use the `master` branch, but you can specify a branch or a tag like so `username/repo#branch`. You may also use the `--clone` flag so your SSH keys are used (allowing you to access your private repositories).

<a name="local"></a>
## Local Craft Kits
You may also use a local craft kit.
```
$ mason craft ~/path/to/my/craft-kit
```

<a name="creating"></a>
## Creating a Craft Kit
Craft kits are light weight javascript applications. They require one `index.js` file as an entry point which exposes the craft kit to the CLI. Beyond that, you can structure it however you see fit.

A nice example is the [laravel craft kit](https://github.com/CodemasonHQ/craft-kit-laravel).

### index.js
| Property | Description                                                                       |
| -------- | --------------------------------------------------------------------------------- |
| name     | *[string]* Simple name for your craft kit                                         |
| default  | *[array]* Default containers for craft kit to use                                 | 
| masonJson | *[object]* [Mason JSON](http://mason.ci/docs/mason-json) for available containers |

### Example index.js
```javascript
module.exports = {
  name: 'laravel',
  default: ["php", "mysql"],
  masonJson: {
  	php: require('./mason-json/php.js'),
  	mysql: require('./mason-json/mysql.js')
  },
}
```
