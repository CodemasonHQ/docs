# Creating an app

- [Introduction](#introduction)
- [Behind the scenes](#behind-the-scenes)
- [Your Git remote](#git-remote)

<a name="introduction"></a>
## Introduction
We measure our success by how easily users can deploy, manage and run their PHP, Node.js, Python, Ruby, Java Clojure, Scala, Go or Docker apps.

On Codemason, every app comes with `git push` deployments via Codemason Git, automated builds and a private image on our Docker registry. Beyond pushing your code, there's not much else for you to worry about. 

Each app is simply a collection of services.

For example, an app for running a simple Laravel project powered by a MySQL database would contain two services. One service for the Laravel project called `web` and another service for the MySQL database called `db`.

Apps can be created via the dashboard or the [Mason CLI](https://codemason.io/docs/mason-cli). When you run the create command, it will prompt you for any information it needs.
```bash
$ mason create quickstart

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote codemason
```


<a name="behind-the-scenes"></a>
## Behind the scenes 
When you run the `create` command, the CLI will create a `git remote` pointing to the Codemason Git repository that was created. This is how you sync your code to Codemason.

Now when you push your code to the Codemason Git remote (`git push codemason master`), it is picked up by our GitLab CI Runner and runs the build instructions in your `.gitlab-ci.yml` file.

Your app will be built into a Docker image and pushed to our private Docker registry. This is all automatic and requires no extra work from you. The image address is available via the Deploy tab of your app and follows the format:
```text
registry.mason.ci/<team-slug>/<app-name>
```

<a name="git-remote"></a>
## Your Git remote 
Upon authentication, the CLI uploads your public SSH key to the Codemason Git. Your SSH key is used for authentication by the git remote, so you won't need to login to anything else. 

By default the Git remote is added as `codemason`. You can list the git remotes for your repository with 
```bash
$ git remote -v 
```

You can change the git remote that's created by setting the `--remote` flag when you run the `create` command.
```bash
$ mason create hello-world --remote production
```

If you wish to prevent the CLI from adding a git remote, you can disable it by setting the `--no-remote` flag.
```bash
$ mason create hello-world --no-remote
```

When you push your code to the git remote (`git push codemason master`), it fires the GitLab CI Runner which runs your tests defined in your `.gitlab-ci.yml`. The build trace will be logged to your terminal. The resulting image is pushed to the Codemason Docker registry and will be automatically available for you to use. 