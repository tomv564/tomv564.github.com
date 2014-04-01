---
layout: post
title: "Git push deployment on IIS"
date: 2014-03-28 18:49:41 +0100
comments: true
categories: 
---

Now that I'm not just slinging C# all day, there are some interesting new problems. 
Like git repositories full of PHP that need to be deployed to Windows and Ubuntu boxes on a whim. 
Currently, we're using WinMerge or even Filezilla to push invidual files across, and I yearn to be able to just do this:

`git push live `

On the Ubuntu servers, [this is easy]](http://mikeeverhart.net/git/using-git-to-deploy-code/)

On Windows, 

1. Follow the [installation instructions](http://bonobogitserver.com/install/) to install Bonobo Git Server 
2. Create a repository for yourapp
3. Create a web application for yourapp, host it under eg. **c:\inetpub\yourapp**
4. Grant the **IIS_IUSRS** group write permissions to this directory.
5. Locate your repository in Bonobo's **App_Data** folder and add the following in **hooks\post-receive**

```
#!/bin/sh
git checkout -f  
```

6. Also edit the config of the repository

```
[core]
  bare = false
  worktree = /inetpub/yourapp
[recieve]
  denycurrentworktree = ignore
```

7. Finally, add the remote to your local git repository: 

``` git remote add http://admin@mywindowsserver/yourapp.git ```