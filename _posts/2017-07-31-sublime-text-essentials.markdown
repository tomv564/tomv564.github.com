---
title: Sublime Text Essentials
layout: post
---

This is my base setup before adding any language-specific tooling:

#### 1. Package Control

The package manager that should by rights be bundled with the editor. 
[Installation instructions here](https://packagecontrol.io/installation)

#### 2. SideBarTools ([github](https://github.com/braver/SideBarTools))

The side bar is incomplete without move / duplicate / delete operations in the context menu. Still looking for a quick shortcut to exclude folders from a project.

**Note**: SidebarEnhancements is often recommended for this, but it was found to have [unexpected telemetry included](https://forum.sublimetext.com/t/rfc-default-package-control-channel-and-package-telemetry/30157).


#### 3. GitGutter ([github](https://github.com/jisaacks/GitGutter))

Classic plugin, clear visible icons in the gutter.

Configuration: (Not sold on the theme yet).
```
"theme": "Bars.gitgutter-theme",
"show_status_bar_text": false
```

#### 4. GitSavvy ([github](https://github.com/divmain/GitSavvy))

Fulfilled my wish of diffing and selective staging from while never leaving the editor.

Memorizable and quick keyboard-driven flow:

1. Open the dashboard with the `git status` command,
2. `,` and `.` to select files
3.  `S`tage/`U`nstage whole files,
4. Inline diff (`L`) + stage `H`unks, 
5. Enter the `C`ommit view that includes the complete diff,
6. `Cmd+enter` to commit,
7. `P` to push to remote. 

Configuration:
```
    "show_commit_diff": true,
    "inline_diff_auto_scroll": true,
    "show_panel_for": ["push, pull, rebase"],
    "git_status_in_status_bar": false
```


#### 5. Theme: El Capitan and Oceanic Next

![screenshot](/uploads/theme_and_scheme.png)
