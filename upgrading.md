# Upgrading an existing app

- [Introduction](#introduction)
- [Upgrading](#upgrading)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
By this stage, you will have already seen how easy it is to deploy an application Codemason. Now we'll show you just how easy it is to push your changes to Codemason an upgrade your app.

Simply modify your application as you would normally and ensure you add the modified files to git
```
$ git add .
$ git commit -m "Update app"
```

Then push it to the Codemason Git remote
```
$ git push codemason master
```

This will trigger our CI runners and build an image with your changes in it as per the build instructions in your `gitlab-ci.yml` file.


<a name="upgrading"></a>
## Upgrading 
Run the `services:upgrade` command to update an existing application. Be sure to specify the service you wish to upgrade in the following format `application/service`
```
$ mason services:upgrade pebble/web 

Upgrading service on Codemason... Done
```

Codemason will upgrade your application. If you are satisfied with your changes you can `finish` the upgrade
```
$ mason services:upgrade pebble/web --finish
```

If you are not happy with your upgrade, you can `rollback`
```
$ mason services:upgrade pebble/web --rollback
```

Additionally, you may `--cancel` upgrades and `--cancel-rollbacks` if required.


<a name="next-steps"></a>
## Next steps
Now you have got the basics of deployment down, let's learn about [managing your environments](/docs/{{version}}/environments) and [scaling](/docs/{{version}}/scaling).
