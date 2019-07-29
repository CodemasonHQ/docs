# Managing multiple environments

- [Introduction](#introduction)
- [Codemason CLI support](#cli-support)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction

Running your app in multiple environments is a great way to help prevent your app breaking in production.

<a name="cli-support"></a>
## Creating a staging environment

You can create a new app to represent your staging environment with the Codemason CLI

```bash
$ mason create --remote staging

Creating app on Codemason...
 ... Created application
 ... Created remote repository
 ... Added git remote staging
```

By default the `mason create` command will create a Git remote named `codemason`. In the command above, we are 
using the `--remote` flag to specify the name `staging` for this remote. Accordingly, you should use 
the following command to push code to your new staging environment:

```bash
$ git push staging master
```