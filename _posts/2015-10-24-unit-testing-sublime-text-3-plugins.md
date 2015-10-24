---
layout: post
title: "Unit testing your Sublime Text 3 plugin"
date: 2015-10-24 00:00:00 +0200
comments: true
categories:
published: true
---

One advantage of developing in Sublime Text is that you end up sharing the same IDE backends with **vim**, **emacs** and **atom** users. 

The Sublime plugins for these backends could be considered little more than glue code. I believe they are worth investing extra time in: They can make or ruin the user experience. 

That said, the development experience isn't great. Your debugging options are:

* Sprinkle `print()` statements everywhere, restart editor
* Install and trigger legitimate debugger (see [this package](https://packagecontrol.io/packages/Plugin%20Debugger))

To enable quick iterative development, here follows a short guide on setting up your package for unit testing. 

### Unit Testing your Sublime Text plugins

The following assumes you have set up a virtualenv (python 3.3 to match ST3), and are working inside the `Packages/YourPackage` directory.

#### Create your first test

* Create a 'test' subfolder 
* Add a test file to it (`test/test_myplugin.py`):

```python
import unittest
import myplugin

class LoadPluginTest(unittest.TestCase):

	def test_can_load(self):
		myplugin.plugin_loaded()
		
```

* Run this test using the ide or the command line: `python -m unittest`

You should see errors about missing types from `sublime` and `sublime_plugin`. We will fix those next:

#### Fix sublime and sublime_plugin imports

Create two stubs: `test/stubs/sublime.py` and `test/stubs/sublime_plugin.py`. 

For inspiration, see 
[some simple stubs](https://github.com/lukexi/stack-ide-sublime/tree/master/test/stubs)
or a [more complete example](https://github.com/spywhere/Javatar/tree/master/tests/stubs)

Import these stubs wherever your plugin fails to load:

```python
try:
    import sublime
except ImportError:
    from test.stubs import sublime

```

#### Fix your own imports


Sublime Text 3 loads your package from it's `Packages/` directory, while your tests run from `Packages/YourPackage`. The import statements are incompatible when running tests:

```python
from YourPackage.yourmodule import function, class
# results in ImportError: No module named YourPackage

from .yourmodule import function, class
# results in SystemError: Parent module '' not loaded, cannot perform relative import

```

I found [this solution](https://github.com/SublimeText/CTags/commit/48f1cd3bcfb8928f4202f43954617029e09aad4c) in the Sublime ctags plugin.

```python
# add YourPackage/ to the import path.
import sys, os
sys.path.append(os.path.dirname(os.path.realpath(__file__)))

# rest of imports in the usual way:
from yourmodule import function, class
```

Not every module needs this logic, but it is best to convert them all to normal imports and then fix loading errors in sublime by adding the above.

#### Fix remaining missing dependencies

Once your plugin loads correctly under test, you can start testing other components, using a combination of more **stubs** and [unittest.mock](https://docs.python.org/3/library/unittest.mock-examples.html) 

#### Anaconda testing setup

The [Anaconda](https://github.com/DamnWidget/anaconda) sublime package has some handy testing shortcuts.  
You'll need a YourPackage.sublime-project, with the following settings (adjust the virtualenv path):

```yaml
{
	# other settings
	"settings":
		{
			"test_command": "python -m unittest",
      		"test_virtualenv": "~/.virtualenvs/python3"
		}
	}
}
```

#### Travis CI and Coveralls in 3 steps

* Sign up for [Travis CI](http://travis.io)
* Sign up for [Coveralls](http://coveralls.io)
* Add this `.travis.yml` to the root of your package:

```yaml
language: python
python:
  - "3.3"

# command to install dependencies
install:
  - pip install coverage
  - pip install coveralls

# command to run tests
script: coverage run --source="." --omit="test/*" -m unittest

# publish results
after_success: coveralls

```

