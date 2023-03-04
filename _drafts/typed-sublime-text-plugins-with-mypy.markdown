---
title: Typed Sublime Text plugins with mypy
layout: post
---

## Generate sublime stubs

Clone the files:

https://github.com/twolfson/sublime-files

Run stubgen:

```
cd sublime-files
stubgen --no-import sublime
stubgen --no-import sublime_plugin
```

sublime_api will be missing, resulting in a lot of `Any`s.
Perhaps pre-writing a stub for it will result in bettter stubs for sublime and sublime_plugin?

Put the resulting `sublime.pyi` files into your plugin's source tree, in a `stubs` subdirectory.

## Starting with mypy

Add the following ini

```
[mypy]
# ignore_missing_imports = True
check_untyped_defs = True
strict_optional = True 
mypy_path = stubs
```

The **mypy_path** setting points mypy to your stubs (otherwise you will see import errors).
**strict_optional** is callers are forced to deal with you returning None sometimes
By default mypy skips untyped code, **check_untyped_defs** is needed to show places to start adding type annotations. 