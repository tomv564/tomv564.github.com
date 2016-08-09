---
layout: post
title: "Git push deployment on IIS"
date: 2014-03-28 18:49:41 +0100
comments: true
categories: 

---

Now that I'm not just slinging C# all day, there are some interesting new problems. 
Like PHP hotfixes that need to be deployed to Windows and Ubuntu boxes on a whim. 
Currently, we're using WinMerge or even Filezilla to push invidual files across, and I yearn to be able to just do this:

`git push live `

On the Ubuntu servers, [this is easy](http://mikeeverhart.net/git/using-git-to-deploy-code/)

On Windows, 

- Follow the [installation instructions](http://bonobogitserver.com/install/) to install Bonobo Git Server 
- Create a repository for yourapp in Bonobo
- Create a web application in IIS for yourapp, under eg. **c:\inetpub\yourapp**
- Grant the **IIS_IUSRS** group write permissions to this directory.
- Locate your repository in Bonobo's **App_Data** folder and add the following in **hooks\post-receive**

```
#!/bin/sh
git checkout -f  
```

Also edit the config of the repository, making sure at least the following settings are present:

```
[core]
  bare = false
  worktree = /inetpub/yourapp
[receive]
  denyCurrentBranch = ignore
```

Finally, add the remote to your local git repository: 

` git remote add yourserver http://admin@yourwindowsserver/yourapp.git `

You're good to go:

` git push yourserver `

