# Command Line Authentication

- [Introduction](#introduction)
- [Authenticating with the CLI](#authentication-with-the-cli)
- [Retrieving current user](#retrieve-current-user)
- [Retrieving your API token](#retrieve-your-api-token)
- [Retrieving current user](#retrieve-current-user)
- [Logging out](#logging-out)

<a name="introduction"></a>
## Introduction
The Mason CLI utilises 3 kinds of authentication, email and password, API token and SSH key. As a user, you will only need to worry about logging in with your email and password. 

When you login, the CLI will obtain an API token. This API token is used to authenticate all of the Codemason API requests. You can view the API Tokens generated for your account in settings under the API tab. 

<a name="authentication-with-the-cli"></a>
## Authenticating with the CLI
Sign in to Codemason account with the `login` command. The CLI will prompt you for your Codemason email and password. 
```
$ mason login
```

<a name="retrieve-current-user"></a>
## Retrieving current user
You can retrieve information about the currently logged in user with the `whoami` command. 
```
$ mason whoami 
```

<a name="retrieve-your-api-token"></a>
## Retrieving your API token 
Retrieve the API token the CLI is using to authenticate your requests 
```
$ mason token 
```

<a name="logging-out"></a>
## Logging out
You can easily sign out of your Codemason account with the `logout` command 
```
$ mason logout 
```
