# Building your Docker image

- [Introduction](#introduction)
- [Behind the scenes](#behind-the-scenes)
- [Next steps](#next-steps)

<a name="introduction"></a>
## Introduction
For those familiar with Docker, you’ll know that images are the cornerstone to containerization. For the uninitiated, images are an immutable snapshot of a container which can be run anywhere - they are what makes containers so portable.

Building an image is a critical step when working with Docker and while it’s a piece of cake doing it locally, things get a little bit more complicated when you try to move from development to deployment.

The good news is, with Codemason, you don’t have to worry about setting up CI runners, building your image and pushing it to a private registry. It all just happens when you commit your code and push it to your Codemason Git remote with `git push codemason master`.

<a name="behind-the-scenes"></a>
## Behind the scenes
The whole point of Codemason is for us to take an opinionated approach to doing things behind the scenes so you don’t have to worry about it and you can focus on building  amazing apps.

When you `mason craft` your application, a `.gitlab-ci.yml` file is added to your application. This is so that when you commit your code and push it to your Codemason Git remote with `git push codemason master`, our GitLab Runner picks that up, runs the build script and pushes it to our private Docker Registry.


<a name="next-steps"></a>
## Next steps
Now you’re familiar with the build process, it’s time to [deploy your app](/docs/{{version}}/deploying-apps).
