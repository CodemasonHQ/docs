# Creating an app

- [Introduction](#introduction)
- [Behind the scenes](#behind-the-scenes)
- [Your Git remote](#git-remote)

<a name="introduction"></a>
## Introduction
On Codemason, an application is simply a collection of services. 

For example, an application for running a simple Laravel project powered by a MySQL database would contain two services. One service for the Laravel project called `web` and another service for the MySQL database called `database`.

The create command is opinionated. 

In addition to creating an application on Codemason, it will also create a new Codemason Git service so you can deploy your code to Codemason. The end result is the same as creating a Codemason Git service from the catalog and following the setup steps.

When you run the create command, it will prompt you for any information it needs.
```
$ mason create 

   Creating application on Codemason...

⁣     Application name (quickstart)
⁣         Service name (web)
⁣         Service path (/Users/ben/quickstart)

   ✔ Created application
   ✔ Created remote repository
   ✔ Added git remote codemason
```

You can also pass values into the command as parameters 
```
$ mason create --application pebble --service laravel --path /Users/git/pebble
```

After you have created your app, you will need you will need to commit the changes the CLI made to your `.gitlab-ci.yml` file
```
$ git add .
$ git commit -m "Build script"
```

<a name="behind-the-scenes"></a>
## Behind the scenes 
Naming things can often be more painful than it really should be, so when you run the create command, the CLI will suggest a randomly generated name for you.

Once you have filled out all relevant information, the CLI will then create a git remote pointing to the Codemason Git and update your `.gitlab-ci.yml` file to point to the correct location on the Codemason registry. 

Now when you push your code to the Codemason Git remote, it is picked up by our GitLab CI Runner and runs the build instructions in your `.gitlab-ci.yml` file.

<a name="git-remote"></a>
## Your Git remote 
Upon authentication, the CLI uploads your public SSH key to the Codemason Git. Your SSH key is used for authentication by the git remote, so you won't need to login to anything else. 

By default the Git remote is added as `codemason`. You can list the git remotes for your repository with 
```
$ git remote -v 
```

When you push your code to the git remote. (`git push codemason master`), it fires the GitLab CI Runner which runs your tests defined in your `.gitlab-ci.yml`. The resulting image is pushed to the Codemason Docker registry. 
```
$ git push codemason master
```

