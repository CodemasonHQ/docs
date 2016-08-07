# Introduction

- [Mason JSON Reference](#mason-json-reference)
  - [Name](#mason-json-name)
  - [Description](#mason-json-description)
  - [Keywords](#mason-json-keywords)
  - [Website](#mason-json-website)
  - [Repository](#mason-json-repository)
  - [Type](#mason-json-type)
  - [Mason Version](#mason-json-mason-version)
  - [Image](#mason-json-image)
  - [Command](#mason-json-command)
  - [Ports](#mason-json-ports)
  - [Environment](#mason-json-environment)
  - [Links](#mason-json-links)
  - [Questions](#mason-json-questions)
  - [Registries](#mason-json-registries)
- [Example Application Mason JSON](#application-example)
- [Example Service Mason JSON](#service-example)
- [Example Instance Mason JSON](#instance-example)

<a name="mason-json-reference"></a>
#  Mason JSON Reference
The Mason JSON is a JSON Schema for defining services, networks and volumes in Codemason. The Schema is deliberately and heavily modelled off the docker-compose.yml file.

Although it's very similar, it also adds functionality beyond what is capable with a standard `docker-compose.yml` file.

Additional capabilities include:
- Questions - accept user input
- Registries - connect to private registries
- Types - define instances, services and applications
- Standard format makes programatically dealing with container orchestration a breeze

Eventually your Mason JSON gets parsed by an internal service at Codemason we call Mortar.

Mortar takes the Mason JSON and validates it. Once it’s been validated it "pre-processes" it - it figures out exactly what type of Mason JSON we're dealing with, responds accordingly and handles service discovery (find or create dependencies). Finally Mortar provisions your instances and/or services.
> mortar: a substance used in building for joining bricks or stones

<a name="mason-json-name"></a>
## Name
*[string, required]*
A simple name for this application, service or instance

<a name="mason-json-description"></a>
## Description
*[string, optional]* 
A short description of what your application, service or instance is

<a name="mason-json-keywords"></a>
## Keywords
*[array, optional]* 
Array of keywords related to this application

<a name="mason-json-website"></a>
## Website 
*[string, optional]* 
Link to the projects website

<a name="mason-json-repository"></a>
## Repository
*[string, optional]* 
Link to the repository containing the related source code

<a name="mason-json-type"></a>
## Type
*[string, required]*
Describes the type of resource we are dealing with. Currently `instance`, `service` and `application` are they only available types.

```json
{
    "type": "instance",
}
```
<a name="mason-json-version"></a>
## Mason Version
*[string, required]*
Identifies how your Mason JSON file should be parsed.
```json 
{
    "masonVersion": "v1", 
}
```

<a name="mason-json-image"></a>
## Image
*[string, optional]*
Specify the Docker image to start the container from. This provides the framework and runtime support for your applications.
```json 
{
    "image": "masonci/php:v5.6-apache",
}
```

<a name="mason-json-command"></a>
## Command
*[string, optional]*
The command that will run to launch and run your application. Our official buildpacks run a default command if this parameter is left empty. However, this option is available if you'd like to get your hands dirty. 
```json
{
    "command": "apache2 -D FOREGROUND"
}
```

<a name="mason-json-ports"></a>
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

<a name="mason-json-environment"></a>
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

<a name="mason-json-links"></a>
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

<a name="mason-json-questions"></a>
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

<a name="mason-json-registries"></a>
## Registries
*[array, optional]* 

### Example
```

{
    "registries": [
        // ...
    ],
}
```

<a name="application-example"></a>
# Application Mason JSON
The Mason JSON file is so powerful, you can use it to define the architecture of your entire application.
To do this, simply define the `instances` and `services` your application requires. Instances and services accept the same schema as defined in this reference document. 

## Example 
In this example, we are creating a simple PHP application and connecting it with a PostgreSQL database. 

```json
{
    "name": "pebble",
    "description": "A demo app",
    "keywords": ["codemason", "demo", "php"],
    "repository": "https://github.com/benmag/pebble",
    "masonVersion": "v1",

    "instances": [
        {
            "name": "app",
            "description": "Web server",
            "image": "codemasonhq/php",
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

<a name="services-example"></a>
# Service Mason JSON
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

<a name="instances-example"></a>
# Instance Mason JSON
An `instance` is your main process, it's what runs you application.

## Example
```json
{
    "name": "web",
    "description": "Web server",
    "image": "codemasonhq/php",
    "type": "service",
    "masonVersion": "v1",
}
```