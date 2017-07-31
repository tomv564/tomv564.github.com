---
title: Sublime Text Essentials
layout: post
---

Visiting colleague's desks, I've seen many a bare-bones unregistered Sublime Text 3 used for editing the odd config file. Unfortunately ST3 does little onboarding to tempt the newcomer further. 

If you are a motivated learner, you are well-served by [this 4-page visual introduction](https://scotch.io/bar-talk/the-complete-visual-guide-to-sublime-text-3-getting-started-and-keyboard-shortcuts) or even [a whole book](https://sublimetextbook.com).

If instead you want to piecemeal your ST3 configuration or get some ideas from a fellow user, follow along on the next couple of posts!

### The two essential packages

What gives ST3 its near-instant startup and responsiveness is the cross-platform C++ core. But some of the base functionality runs in an embedded python engine, and this is also how ST3 can load plugins (officially called *Packages*).

#### 1. Package Control

Believe it or not, ST3 does not ship with a Package installer by default!
Package control gives you:

* A searchable package index containing tools, hacks and themes
* Automatic updates of all your installed packages

[Installation instructions here](https://packagecontrol.io/installation)

After installing this, you can find the next packages by opening the command palette (`cmd+shift+P` or `ctrl+shift+p`) and searching "Install Package"

#### 2. SidebarEnhancements

File operations like *duplicate, delete, rename and move* are all available in the command palette, but when you're mousing around exploring the project in the sidebar, it's super handy to have them on the context-menu.

If you've created a sublime project file, there are some handy shortcuts under the **Project** sub-menu. For example, exclude that `node_modules` folder from the file switcher by choosing **Hide from Sidebar**.

#### More to come

I will have something for the git users and the javascript/typescript developers coming up!

Time willing, there will be more in the future on:

* Build systems
* Python development
* Scala development
* Sublime Text Package development
