# Deploying your app

- [Introduction](#introduction)
- [Deployment](#deployment)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
By this stage you will have already prepared your app by initialising your git repository, created your application and prepared your Codemason git remote with the create command. 

Pushing your code to the Codemason git remote will trigger our CI Runner. This will run the build instructions in you `.gitlab-ci.yml` file. 


<a name="deployment"></a>
## Deployment 
Deploy your application to your server with the `deploy` command and the application you wish to deploy to as a parameter. 
```
$ mason deploy --to applicationName 
```

Committing and pushing your code to the Codemason Git remote gets it built into a Docker image, while deploying it puts it on your server for use.  

When you are ready to actually spin up the services your application will use on your server, you can do that via the Codemason Web UI or the `deploy` command. 

The `deploy` command will also fire a `git push codemason master` for you, so you can be sure your latest changes have been built into the image.  

<a name="next-steps"></a>
## Next steps
Now you have successfully deployed your application, we'll show you what's involved in [upgrading an application](/docs/{{version}}/upgrading). 
