---
layout: post
title: "Maintaining legacy Yii PHP apps"
date: 2014-06-17 00:00:41 +0200
comments: true
categories:
published: false
---

Yii is a sort of Ruby on Rails clone. You get some of the good features (migrations for example), but not the amazing language or community.

The application architecture is probably good enough for simple forms-to-mysql webapps. Unfortunately the framework provides little guidance once complexity grows, so be prepared for monster models and controllers. Tooling isn't great either, so soon I found myself littering var_dumps all over the code and F5-ing in the browser like it's 1998.

### So what can you do?

**Automate deployment** because it's so easy with PHP. We stopped working on the server and now push to a bare git repository with a post-receive hook to checkout and run migrations. If you deploy PHP applications to IIS, you can use a similar setup [I wrote about before](/blog/2014/03/28/git-push-deployment-on-iis/).

**Set up a build server**. I like TeamCity with the PHP meta-runners, but Travis CI is an option if your source is public or you have money.
Even if there are no tests, you can run PHP lint on the whole directory. You can catch some real stupid errors this way.

**Make unit testing cheap**. Document how to set up the right PHPUnit ([YII is picky](https://github.com/yiisoft/yii/issues/1563)), and make it only a keystroke/click away. (Psst, using Sublime Text? I have a [Yii-friendly version of the SimplePHPUnit plugin](https://github.com/tomv564/SimplePHPUnit))

**Finally, run the tests on the build server**.
If your migration history is incomplete like ours was, you may also have to set up a set up a minimal database build. We keep a mysqldump the structure of a recent database on the build server. Every build this database is restored, followed by a `yiic migrate`.

### What does it get you?

Having this set up has not allowed significant refactorings or converted us to TDD. But the last few months I've noticed some changes of habit:

It's more worthwhile to fix little bugs and annoyances because deployments are safe and fast. As a bonus, the fixes also always make it into version control.

We've also had some valuable fixes sitting in still-dodgy code. After writing some tests, I felt safe pushing it live. Any new bugs would be triaged quickly because *writing more tests was now cheap.*



