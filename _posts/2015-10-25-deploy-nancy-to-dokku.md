---
layout: post
title: "Deploying a Nancy app to Dokku"
date: 2015-10-25 00:00:00 +0200
comments: true
categories:
published: true
---

After building a .NET Nancy toy project in Visual Studio, I decided to test the container deployment story with [Dokku](http://progrium.viewdocs.io/dokku/). Dokku is essentially a private Heroku, and it uses Heroku's buildpacks to convert your source code into a docker image.

There are quite a few outdated guides and buildpacks out there, I will direct you to [this working buildpack](https://github.com/friism/heroku-buildpack-mono/) for Mono 3, and these more recent [buildpacks for Mono 4](https://elements.heroku.com/buildpacks/adamburgess/heroku-buildpack-mono
).

Here follows the recipe:

### Get a working Mono build (preferably on *nix)

You will need to [install mono](http://www.mono-project.com/docs/getting-started/install/) if you haven't done so yet.

If you need to restore your packages on your unix system, you can set up the nuget command line client:

* [Install NuGet](http://headsigned.com/article/running-nuget-command-line-on-linux) (read instructions carefully and grab their linked Microsoft.Build.dll file)
* Use [this handy gist](https://gist.github.com/andypiper/2636885) to set up a wrist-friendly shortcut.

Now try running `xbuild` in your project root directory. It should detect your .sln file. You might encounter the following problems with Visual Studio projects:

* MSTest projects don't compile out of the box (missing Microsoft.VisualStudio.TestTools.UnitTesting.dll). You can temporarily unload/remove them from the solution or use NUnit instead.

* Mono 3 chokes on newer Visual Studio ToolsVersion attributes like "v12.0" in the csproj files. Changing them to "4.0" is enough to get it working.


### 12-factor your configuration

You can't/shouldn't log into a running container to tweak the configuration files. Instead, follow the [12-factor-app principles](http://12factor.net) and provide settings via environment variables.

Dokku / Heroku both inject a PORT variable (usually 5000) for your application to bind to in the environment. Make sure your Nancy app binds to PORT on the name localhost.

Check your web.config for any settings that are environment-specific (eg. connection strings), and load them from the environment instead.

### Prepare Dokku

Log onto your Dokku server, and start by creating the app (the name **myapp** becomes *myapp.mydokku.com*):

`dokku apps:create myapp`

Set the buildpack parameter for Dokku:

`dokku config:set myapp BUILDPACK=https://github.com/friism/heroku-buildpack-mono/`

### Configure your project for the buildpack

Buildpacks look for a **Procfile** to determine how to launch your app. Place a Procfile in the root directory of your project with these contents:

```
web: mono YourConsoleApp.exe
```

No path should be necessary, the buildpack is fairly smart about gathering your build outputs.

### Push to dokku

Make sure your last changes are committed, then set up a remote and push:

```
git remote add dokku dokku@mydokku.com:myapp
git push dokku master
```

Especially the first push can be quite slow as the buildpack downloads a 100mb+ mono distribution. Read carefully for clues if the container fails to start.

If the container fails the checks, make sure that Nancy is actually configured to bind to the URI you have specified (eg `http://localhost:5000/`)

### Conclusion

See the [full source](https://github.com/tomv564/sms-demo) and specifically [Program.cs](https://github.com/tomv564/sms-demo/blob/master/SMSServer/Program.cs) for details on launching the Nancy process.