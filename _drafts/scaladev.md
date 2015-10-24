---
layout: post
title: "Setting up Sublime Text 3 for Scala development"
date: 2015-10-08 00:00:00 +0200
comments: true
categories:
published: false
---

See: [Recent blog post](http://blog.adilakhter.com/2015/07/12/setup-ensime-with-sublime-in-os-x-scala-development-with-sublime/)

Global ensime sbt plugin (quotes missing on blog post!)

```
$ cat ~/.sbt/0.13/plugins/plugins.sbt
addSbtPlugin("org.ensime" % "ensime-sbt" % "0.2.0")
```

Generate some shizz in your project??


`sbt gen-ensime` 


Launch the server


`brew install coreutils`



Starting the server takes forever and is a pain

* Use a Start Server command to call the sh??



Need to see logs somewhere because you will wonder if it's running...

`tail -d .ensime_cache/Resolution/sbt.log`