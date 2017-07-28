---
title: Javascript & Typescript services for Sublime
layout: post
---

Time for a 2017 update to the [previous guide](setting-up-sublime-text-3-for-es6-development.html) to Sublime Text for ES6 javascript. 

#### Microsoft's TypeScript Plugin

Typescript came, and Microsoft did its best to promote it by writing an impressive [Sublime Text plugin](https://github.com/Microsoft/TypeScript-Sublime-Plugin/). 

It can be made to support plain javascript. [This issue](https://github.com/Microsoft/TypeScript-Sublime-Plugin/issues/474) contained some hints, and I added additional overrides in [my fork](https://github.com/tomv564/TypeScript-Sublime-Plugin/)

The development has slowed down a bit, but I think it's hard to improve on this setup.

#### Sourcegraph's language server

A whole ecosystem of editor-independent language servers has sprung up, united by Microsoft's Language Server Protocol. You can see a full list at [langserver.org](http://langserver.org), the [js/ts server](https://github.com/sourcegraph/javascript-typescript-langserver) being written by SourceGraph.

Although Sourcegraph also started an LSP plugin for Sublime Text (by forking the Microsoft's Typescript plugin), this effort appears to have stalled since fall 2016. 

#### Universal LSP support

I've started a fresh effort for a [language-agnostic LSP plugin](https://github.com/tomv564/LSP) for Sublime Text 3. 

The goal is to be as language-agnostic as possible (leaning on the protocol) and to use the best of the ST3 plugin ecosystem to make an experience competitive with Atom/VS Code.

If you like shiny and unstable tools, please feel try it out! Screenshots and proper instructions are forthcoming.
