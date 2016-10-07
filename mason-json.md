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
  - [Entrypoint](#mason-json-entrypoint)
  - [Scale](#mason-json-scale)
  - [Memory](#mason-json-memory)
  - [CPU](#mason-json-cpu)
  - [Privileged](#mason-json-privileged)
  - [Ports](#mason-json-ports)
  - [Environment](#mason-json-environment)
  - [Labels](#mason-json-labels)
  - [Volumes](#mason-json-volumes)
  - [Links](#mason-json-links)
  - [Questions](#mason-json-questions)
  - [Registries](#mason-json-registries)
- [Example Application Mason JSON](#application-example)
- [Example Service Mason JSON](#service-example)

<a name="mason-json-reference"></a>
#  Mason JSON Reference
The Mason JSON is a JSON Schema for defining services, networks and volumes in Codemason. The Schema is deliberately and heavily modelled off the docker-compose.yml file.

Although it's very similar, it also adds functionality beyond what is capable with a standard `docker-compose.yml` file.

Additional capabilities include:
- Questions - accept user input
- Registries - connect to private registries
- Standard format makes programmatically dealing with container orchestration a breeze

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
Describes the type of Mason JSON we are dealing with. Currently `service` and `application` are they only available types.

```json
{
    "type": "service",
}
```
<a name="mason-json-mason-version"></a>
## Mason Version
*[string, required]*
Identifies how your Mason JSON should be parsed.
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
    "image": "codemasonhq/php",
}
```

<a name="mason-json-command"></a>
## Command
*[string, optional]*
The command that will run to launch and run your application. Our official buildpacks run a default command if this parameter is left empty. However, this option is available if you'd like to get your hands dirty. 
```json
{
    "command": "apache2 -D FOREGROUND",
}
```

<a name="mason-json-entrypoint"></a>
## Entrypoint
*[string, optional]*
Set the entrypoint for your container. The entrypoint is prefixed to any command you run through the container.
```json
{
  "entrypoint": "command param1 param2",
}
```

<a href="mason-json-scale"></a>
## Scale
*[integer, optional]*
Set the number of containers to spin up.
```json
{
  "scale": 1,
}
```

<a name="mason-json-memory"></a>
## Memory
*[string, optional]*
Set the memory limit for the container.
```json
{
  "memory": "128mb",
}
```

<a name="mason-json-cpu"></a>
## CPU
*[string, optional]*
The value of shares of the CPU available to the container.
```json
{
  "cpu": "04",
}
```
<a name="mason-json-privileged"></a>
## Privileged
*[bool, optional]*
Grant extended privileges to the container.
```json
{
  "privileged": false,
}
```

<a name="mason-json-ports"></a>
## Ports
*[array, optional]*
Define the ports to expose. You may specify specific ports (`HostPort:ContainerPort`) or just specify the container port and a random host port will be allocated. 
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
Set environment variables.
```json
{
    "environment": {
        "POSTGRES_USER": "admin",
        "POSTGRES_PASSWORD": "mypass",
        "POSTGRES_DB": "test"
    }   
}
```

<a name="mason-json-labels"></a>
## Labels
Set labels.
*[object, optional]*
```json
{
  "labels": {
    "com.example.foo": "bar"
  }
}
```

<a name="mason-json-volumes"></a>
## Volumes
*[array, optional]*
Declare volumes to mount.
```json 
{
  "volumes": [
    "./data/mysql:/var/lib/mysql:rw"
  ]
}
```

<a name="mason-json-links"></a>
## Links
*[array, optional]*
Define links to other services.
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
Through Mason JSON, you can also access your private registries and do awesome things *privately*.

### Example
```
{
    "registries": [
        {
            "address": "registry.example.com",
            "email": "foo@example.com",
            "username": "foo",
            "password": "bar123"
        }
    ],
}
```

<a name="application-example"></a>
# Application Mason JSON
Mason JSON is so powerful, you can use it to define the architecture of your entire application.
To do this, simply define the `services` your application requires. Services accept the same schema as defined in this reference document. 

## Example 
In this example, we are creating a simple PHP application and connecting it with a PostgreSQL database. 

```json
{
    "name": "pebble",
    "description": "A demo app",
    "keywords": ["codemason", "demo", "php"],
    "repository": "https://github.com/benmag/pebble",
    "masonVersion": "v1",
    "services": [
        {
            "name": "app",
            "description": "Web server",
            "image": "codemasonhq/php",
            "links": [
                "postgres"
            ]
        },
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
You can also just define a single service in a similar fashion with one minor additional requirement. You must define it's type as a service (`type: "service"`).

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