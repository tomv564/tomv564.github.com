---
title: Javascript & Typescript in Sublime Text 3
layout: post
---

#### Sublime syntax files, more than just pretty colors

For modern flavours Javascript, the `Babel` package does the job well.
You need to manually set the syntax to `Javascript (Babel)` and choose **Syntax** -> **Open all with current extension as...**

The Typescript syntax from Microsoft unfortunately breaks the syntax-based *List symbols* (`cmd+r`) and *Jump to symbol* (`cmd+option+down`) commands. 

![](/uploads/typescript_syntax_broken_definitions.png)
![](/uploads/typescript_syntax_broken_list.png)


[My fork](https://github.com/tomv564/TypeScript-TmLanguage) of the syntax fixes this issue, or you can install [Microsoft's syntax](https://github.com/Microsoft/TypeScript-TmLanguage) if it doesn't bother you.


#### JS/TS Language server

Sourcegraph has built a [javascript/typescript language server](https://github.com/sourcegraph/javascript-typescript-langserver). 

Together with my [LSP package for Sublime Text](https://github.com/tomv564/LSP) it will give you the same completions, diagnostics, quick fixes and refactorings as Visual Studio Code.

#### SublimeLinter with tslint

Microsoft proposes running linters over the same language server protocol. 
Until the ecosystem has caught up, it's probably best to run [SublimeLinter3](https://github.com/SublimeLinter/SublimeLinter3/) with the [SublimeLinter-contrib-tslint](https://github.com/lavrton/SublimeLinter-contrib-tslint
) plugin. 

#### Integration

Although errors in *displayed code* are quite visible, I've wanted a *Problems* panel in Sublime Text for a long time. 
The LSP package adds a *diagnostics panel*, and linter output can be added to it:

![](/uploads/lsp_tslint_diagnostics.png)

