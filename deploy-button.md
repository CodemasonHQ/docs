# Deploy to Codemason button

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Creating the mason.json file](#creating-the-mason-json-file)
- [Testing the mason.json file](#testing-the-mason-json-file)
- [Adding the Codemason button](#adding-the-codemason-button)


<a name="introduction"></a>
## Introduction

The "Deploy to Codemason" button provides a simple one-click option for users to deploy apps on Codemason.

The Codemason button is super versatile and can be used to cover a variety of needs. Here are some examples for how you can use the Codemason button:

* Add it to your project README file for easy deployment
* Add it to the end of your tutorials so people can easily try and build on examples
* Send one to your customers customers/clients so they can easily deploy your work
* ... And more!

It provides a great alternative to maintaining a list of manual steps required to configure an app.

Here's an example of a deploy button for a sample PHP app:

[![Deploy](https://codemason.io/img/deploy.svg)](https://codemason.io/apps/create?template=https://github.com/benmag/slack-invite-automation/)

[Learn more about adding the Codemason button](#adding-the-codemason-button)

<a name="requirements"></a>
## Requirements 
The main requirement for a Codemason button is that your app have a valid `mason.json` file in its root directory and the app source code is hosted in a GitHub repository.


<a name="creating-the-mason-json-file"></a>
## Creating the mason.json file
The `mason.json` file is the designated file to store the [Mason JSON](/docs/{{version}}/mason-json) for your app. 
Mason JSON is a clear and easy to write JSON schema for declaring all the information required to run an app on Codemason

Here's a sample `mason.json` file
```json
{
  "name": "sample-php-app",
  "description": "A demo PHP app",
  "repository": "https://github.com/benmag/slack-invite-automation",
  "masonVersion": "v1",
  "type": "application",
  "services": [
    {
      "name": "web",
      "image": "${app}/${branch}:${commit}"
    }
  ],
  "servers": [
    {
      "size": "standard-1x"
    }
  ]
}
```


<a name="testing-the-mason-json-file"></a>
## Testing the mason.json file
It is a good idea to test your Mason JSON to ensure it is working as you expect.

You can do this via Codemason:
```text
https://codemason.io/apps/create?template=https://github.com/codemasonhq/getting-started-php
```


<a name="adding-the-codemason-button"></a>
## Adding the Codemason button
Once you're happy that your `mason.json` file is valid and working as expected, you can add the button to you app's README.

Be sure to change the `template` query parameter to match the URL of your repository.

The following snippet is an example of how you can add the Codemason button to a document using Markdown
```markdown
[![Deploy](https://codemason.io/img/deploy.svg)](https://codemason.io/apps/create?template=https://github.com/benmag/slack-invite-automation/)
```

If you'd prefer not to use Markdown, here's the equivalent implementation of the Codemason button in HTML
```html
<a href="https://codemason.io/apps/create?template=https://github.com/codemasonhq/getting-started-php">
  <img src="https://codemason.io/img/deploy.svg" alt="Deploy">
</a>
```
