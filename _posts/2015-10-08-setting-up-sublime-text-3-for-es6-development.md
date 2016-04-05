---
layout: post
title: "Setting up Sublime Text 3 for ES6 development"
date: 2015-10-08 00:00:00 +0200
comments: true
categories:
published: true
---

Getting fast in a keyboard-oriented editor can be a big productivity boost, but I'm too sloppy to stop cutting myself repeatedly on javascript's sharp edges. Good linting, autocomplete and testing can save you that trip to the browser or debugger!

Here's a quick recipe to get linting and autocomplete with support for ES6 and JSX.

Tested in OS-X only, assumes you have the following installed already:

* Node
* ST3
* Package Control
* SublimeLinter

## Syntax highlighting

The [Babel Sublime Text package](https://github.com/babel/babel-sublime#installation)  adds Syntax settings with good highlighting for ES6 and JSX. It's a must-have!


## Linting setup

Eslint is the modern linter of choice, but have a look at [all these rules](http
://eslint.org/docs/rules/) and decide if you really want to spend your time configuring them.

[Standard](http://standardjs.com) and [semistandard](https://github.com/Flet/semistandard) wrap eslint with sensible defaults, the former banning semicolons as well.

Make your choice and install it in your project (`--save-dev`) or globally (`-g`)

`npm install --save-dev semistandard`

Then install the matching **SublimeLinter-contrib-standard** or **SublimeLinter-contrib-semistandard** package via Package Control.

In your project you should now see warnings about unused vars, indentation and newlines. If not, check the console ( **ctrl+`** ) and make sure the linter installed correctly!

## Autocompletion

Thanks to Marijnh's hard work, [Tern](http://github.com/marijnh/tern) now supports ES6 features. [Support](https://marijnhaverbeke.nl/fund/) his work!

Start by installing the **tern\_for\_sublime** package via Package Control.

After installation, Sublime should prompt you to install the **tern** node package as well.

If tern has issues launching npm or node on OS-X, it probably has not inherited your path properly. There are two options to fix this:

* Start Sublime from the terminal using `subl`
* Install the [FixMacPath](https://github.com/int3h/SublimeFixMacPath) package.

In the root of your project create a **.tern-project** file with the following contents:

{% highlight javascript %}

{
  "libs": [
    "browser"
  ],
  "plugins": {
     "node": {},
     "modules": {},
     "es_modules": {},
     "jasmine": {}
  }
}

{% endhighlight %}

These settings are documented [here](http://ternjs.net/doc/manual.html#configuration), here is a list of built-in [libs](https://github.com/marijnh/tern/tree/master/defs) and [plugins](https://github.com/marijnh/tern/tree/master/plugins).

You should a list of es6 array functions after typing `[].` in a saved javascript file in your project:

![Screenshot](/images/sublime-tern.png)

## Jasmine testing

You front-end packager (browserify or webpack) should already be using Babel to transpile ES6 for current browser support.

The same transpilation will be necessary to run your jasmine tests. Install **jasmine-es6** to automate this for you:

`npm install jasmine-es6`

In the project's **package.json**, add a `"test": "jasmine"` line under scripts.

Finally, add a build system to Sublime with the following content.

{% highlight yaml %}
{
    "shell_cmd": "npm test",
    "working_dir": "${project_path}",
    "selector": "source.js",
    "file_regex": "^.+\\((.+):([0-9]+):([0-9]+)\\)$"
}

{% endhighlight %}

You should now be able to run your tests with with `super+b` and navigate to errors with `F4`.