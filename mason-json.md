#  Mason JSON Reference
The Mason file is a json file for defining services, networks and volumes in Codemason. This file is deliberately and heavily modelled off the docker-compose.yml file.

Although it's very similar, it also adds functionality beyond what is capable with a standard `docker-compose.yml` file.

Additional capabilities include:

- Questions - accept user input
- Build from repository
- Deploy to Codemason
- Service discovery - finding or creating if required
- Registries - connect to private registries 

Eventually your Mason file gets parsed by an internal service at Codemason we call Mortar.

Mortar takes the `mason.json` file and validates it. Once it’s been validated it “pre-processes” it - it figures out exactly what type of `mason.json` file we’re dealing with, responds accordingly and handles service discovery (find or create dependencies). Finally Mortar provisions your instances and/or services.
> mortar: a substance used in building for joining bricks or stones

## Name
*[string, required]*
A simple name for this `mason.json` file

## Description
*[string, optional]* 
A short description of what this `mason.json` file does

## Keywords
*[string, optional]* 
Array of keywords related to this application

## Website 
*[string, optional]* 
Link to the projects website

## Repository
*[string, optional]* 
Link to the repository containing applications source code

## Type
*[string, optional]*
Describes the type of resource we are dealing with. Currently `instance`, `service` and `application` are they only available types.

```json
{
    "type": "instance",
}
```

## Mason Version
*[string, required]*
Identifies how your `mason.json` file should be parsed.
```json 
{
    "masonVersion": "v1", 
}
```

## Image
*[string, optional]*
Specify the image to start the container from. This provides the framework and runtime support for your applications.
```json 
{
    "image": "masonci/php:v5.6-apache",
}
```

## Build 
*[object, optional]*

### Parameters 
| Name  | Type    | Required | Description  |
| ----- | ------- | -------- | ------------ |
| repository | string         | no | The repository containing the applications source code. |
| commands   | array[string]  | no  | Any commands you want your application to run during the build process. |

### Examples 
**Run build commands**
```json
{
    "build": {
        "commands": [
            "composer install",
            "php artisan migrate",
        ]
    }
}
```

**Retrieve source from repository**
```json
{
    "build": { 
        "repository": "http://github.com/benmag/pebble",
        "commands": [
            "composer install",
            "php artisan migrate",
        ]
    }
}
```


## Command
*[string, optional]*
The command that will run to launch and run your application. Our official buildpacks run a default command if this parameter is left empty. However, this option is available if you'd like to get your hands dirty. 
```json
{
    "command": "apache2 -D FOREGROUND"
}
```

## Ports
*[array, optional]*
Define the ports to expose. You may specify specific ports (`HostPort:ContainerPort`) or just specify the container port and a random host port will be chosen. 
```json
{
    "ports": [
        "5432",
    ],
}
```

## Environment
*[object, optional]*
Set environment variables 
```json
{
    "environment": {
        "POSTGRES_USER": "admin",
        "POSTGRES_PASSWORD": "mypass",
        "POSTGRES_DB": "test"
    }   
}
```

## Links
*[array, optional]*
Define links to other services
```json
{
    "links": [
        "postgres",
    ],
}
```

## Questions
*[array, optional]* 

### Parameters 
| Name  | Type    | Required | Description  |
| ----- | ------- | -------- | ------------ |
| key   | string  | yes  | A `key` is eventually mapped back to an environment variable. |
| value | string  | yes  | A default value. |
| description | string | no | A human friendly explaination of what the value is for. |
| required | bool | no | A bool indicating whether a value is required. Default: `true` |

### Example 
````json
{
    "questions": [
        {
            "key": "POSTGRES_USER", 
            "value": "admin",
            "description": "This variable will create the specified user with superuser power",
            "required": true,
        },
        {
            "key": "POSTGRES_PASSWORD", 
            "value": "",
            "description": "This environment variable sets the superuser password for PostgreSQL",
            "required": true,
        },
        {
            "key": "POSTGRES_DB", 
            "value": "",
            "description": "Default database that is created. If empty, then the value of POSTGRES_USER will be used.",
            "required": false,
        },
    ],
}
````


# Application 
The `mason.json` file is so powerful, you can use it to define the architecture of your entire application.
To do this, simply define the `instances` and `services` your application requires. Instances and services accept the same schema as defined in this reference document. 

## Example 
In this example, we are creating a simple PHP application and connecting it with a PostgreSQL database.
This `mason.json` file can be included 

```json
{
    "name": "pebble",
    "description": "A demo app",
    "keywords": ["codemason", "demo", "php"],
    "repository": "https://github.com/benmag/pebble",
    "masonVersion": "v1",

    "instances": [
        {
            "name": "web",
            "description": "Web server",
            "image": "masonci/php:v5.6-apache",
            "build": { 
                "commands": [
                    "composer install",
                    "php artisan migrate",
                ]
            },
            "links": [
                "postgres"
            ]
        }
    ],

    "services": [
        {
            "name": "postgres",
            "description": "Object relational database",
            "image": "postgres",
            "ports": [
                "5432",
            ],
            "environment": {
                "POSTGRES_USER": "admin",
                "POSTGRES_PASSWORD": "mypass",
                "POSTGRES_DB": "test"
            },
        }
    ],
    
}
```

# Service
You can define a service in a similar fashion with one minor additional requirement. You must define it's type as a service (`type: "service"`)

## Example
```json
{
    "name": "postgres",
    "description": "Object relational database",
    "image": "postgres",
    "type": "service",
    "masonVersion": "v1",
    "ports": [
        "5432",
    ],
    "environment": {
        "POSTGRES_USER": "admin",
        "POSTGRES_PASSWORD": "mypass",
        "POSTGRES_DB": "test"
    },
    "questions": [
            {
                "key": "POSTGRES_USER", 
                "value": "admin",
                "description": "This variable will create the specified user with superuser power",
                "required": true,
            },
            {
                "key": "POSTGRES_PASSWORD", 
                "value": "",
                "description": "This environment variable sets the superuser password for PostgreSQL",
                "required": true,
            },
            {
                "key": "POSTGRES_DB", 
                "value": "",
                "description": "Default database that is created. If empty, then the value of POSTGRES_USER will be used.",
                "required": false,
            },
        ],
}
```

# Instance
An `instance` is your main process, it's what runs you application.

## Example
```json
{
    "name": "web",
    "description": "Web server",
    "image": "masonci/php:v5.6-apache",
    "type": "service",
    "masonVersion": "v1",
    "build": { 
        "commands": [
            "composer install",
            "php artisan migrate",
        ]
    },
}
```

# Registries

## Example
```

{
    "registries": [
        // ...
    ],
}
```