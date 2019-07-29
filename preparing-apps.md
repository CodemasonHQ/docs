# Preparing an app

- [Track your codebase in a Git repository](#git)
- [Add a Codemason Git remote](#remote)
- [Prepare codebase for deployment](#codebase)
- [Use the correct port](#port)
- [Next steps](#next-steps)

<a name="git"></a>
## Track your codebase in a Git repository
Codemason relies heavily on Git, a popular version control system. Before you can deploy your codebase with Codemason, it must be a Git repository. 

You can initialise a git repository in the current folder by running:
```bash
$ git init
```

Once initialised, you can add and commit your files with 
```bash
$ git add .
$ git commit -m "Creating my first Codemason app"
```

Learn more about Git
* [Git installation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [First-time Git setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

<a name="remote"></a>
## Add a Codemason Git remote 

### For a new app
Every Codemason app has it's own Codemason Git repository. You can deploy new versions of your app by pushing your changes to this repo. To do this, you local Git repo needs to have Codemason added as a remote.

The `mason create` command will create a new empty app on Codemason and a corresponding empty Codemason Git repository for your app. The Codemason Git repo will be automatically added as a remote in your local repository.

```bash
$ mason create pebble

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote codemason
```

Confirm that a remote named `codemason` has been set with the `git remote` command
```bash
$ git remote -v
codemason	git@git.mason.ci:my-team/pebble (fetch)
codemason	git@git.mason.ci:my-team/pebble (push)
```

### For an existing app
If you have already created a Codemason app, you can easily add a remote to your local repo with the `mason git:remote` command
```bash
$ mason git:remote pebble
```

<a name="codebase"></a>
## Prepare codebase for deployment
The most important part of preparing your codebase for deployment is building it into a Docker image. Codemason does this by looking for a `.gitlab-ci.yml` and `Dockerfile` in the root directory of your app.

> {tip} You can create these files automatically for most major languages and frameworks using [Craft Kits](/docs/{{version}}/craft-kits)

The `.gitlab-ci.yml` file is what tells Codemason how to build your app. It works just like any normal [GitLab CI/CD Pipeline Configuration](https://docs.gitlab.com/ee/ci/yaml/). 

Here is an example `.gitlab-ci.yml`:
```yaml
stages: 
  - build

services:
 - docker:dind

before_script:
  - docker login -u=gitlab-ci-token -p $CI_BUILD_TOKEN $CI_REGISTRY

build: 
  stage: build
  script: 
    - docker build -t $CI_REGISTRY_IMAGE:latest .
    - docker push $CI_REGISTRY_IMAGE:latest
```

The `Dockerfile` tells Codemason (and Docker) how to run your app. Codemason provides a set of "sensible defaults" for most languages so **you do not need to be familiar with Docker to get started**.

Here is an example `Dockerfile` (PHP): 
```docker
FROM codemasonhq/php
```

[Learn more about Dockerfile](https://docs.docker.com/engine/reference/builder/)

Remember to commit these new files to source control
```bash
$ git add .
$ git commit -m "Preparing codebase for Codemason"
```

<a name="port"></a>
## Use the correct port

By default, each Codemason service gets its own Codemason domain, which follows the format `[service name].[app name].[team].c-m.io`.
Requests to these domains are pointed to the internal port `80`. 
In order to take advantage of this functionality, your service must run on port `80`.
  


<a name="next-steps"></a>
## Next steps
After you have prepared your app, you are ready to [create an app](/docs/{{version}}/creating-apps).