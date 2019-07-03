# Managing environments

- [Introduction](#introduction)
- [Codemason CLI support](#cli-support)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
Environments provide a logical way of ensuring complete separation between different applications. Each team has 3 default environments, `development`, `staging` and `production`. While we have opinionated defaults, you may use the environments however 
you see fit.

If you are unfamiliar with the architecture of a Codemason application, you can find out more about [environments](/docs/{{version}}/architecture#environments) in the [application architecture](/docs/{{version}}/architecture) documentation.

<a name="cli-support"></a>
## Codemason CLI support
By default the Codemason CLI will use the `development` environment. You may provide an environment with the `--environment` flag. 

```bash
$ mason create pebble --environment production
```

<a name="next-steps"></a>
## Next steps
Now your app is up and running with separation between development, staging and production, let's take a look at [scaling](/docs/{{version}}/scaling) with Codemason. 
