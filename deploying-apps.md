# Deploying your app

- [Introduction](#introduction)
- [Deployment](#deployment)
  - [Quick deploy](#quick-deploy)
  - [Deploy services individually (recommended)](#deploy-individually)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
By this stage you will have already [created your app](/docs/{{version}}/creating-apps) and pushed your code, triggering the CI runner.


<a name="deployment"></a>
## Deployment 
<a name="quick-deploy"></a>
### Quick deploy
Use the `deploy` command to quickly deploy your entire app to Codemason in one command. 
```bash
$ mason deploy pebble
```

This will read your `docker-compose.yml` file and create the corresponding services on Codemason.

By default the command will attempt to the `.env` file, if available and pass them as environment variables to the deployment. You can change the env file it loads by setting the `--env-file` flag or disable loading an env file by setting the `--no-env-file` flag.

<a name="deploy-individually"></a>
### Deploy services individually 
You can also deploy individual services using the `services:create` command. This approach is recommended as it gives you the most control over your deployment.

```text
USAGE
  $ mason services:create SERVICE

ARGUMENTS
  SERVICE  service to create formatted as `<app>/<service>`

OPTIONS
  -c, --command=command          command for service to run
  -e, --environment=environment  [default: development] the environment to access
  -i, --image=image              image for service to run
  -l, --link=link                link to another service
  -p, --port=port                ports to define on service
  -v, --volume=volume            volume to mount on service
  --env=env                      env variable available to the service
  --env-file=env-file            path to env file to load

```
The `services:create` command is extremely easy to use due to it's expressive nature. If no `--image` flag is provided, the app image will be used.

```bash
$ mason services:create pebble/web -p 80:80 --env-file .env

Creating service on Codemason...... done

    NAME     IMAGE                                        COMMAND     PORTS
    web      registry.mason.ci/benm/pebble                 80:80
```

The CLI is capable of creating complex services, just like you can via the Codemason UI.
```bash
$ mason services:create pebble/db --image mariadb -p 3306:3306 \
	--env MYSQL_DATABASE=pebble \
	--env MYSQL_USER=demo \
	--env MYSQL_PASSWORD=secret \
	--env MYSQL_ROOT_PASSWORD=supersecret \
	--volume /home/data/mysql/data:/var/lib/mysql
	
	Creating service on Codemason...... done

    NAME     IMAGE       COMMAND     PORTS
    db       mariadb                 3306:3306
```

<a name="next-steps"></a>
## Next steps
Now you have successfully deployed your application, we'll show you what's involved in [upgrading an application](/docs/{{version}}/upgrading). 
