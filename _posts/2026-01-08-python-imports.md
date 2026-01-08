---
layout: post
title: "Python imports: A comprehensive guide"
---

Anyone who worked in Python for a while must have seen at least one script, that
was written in the gist of:

```python
from magical_package import solution

problem = input()
print(solution(problem))
```

In fact, that is also a very easy way to start flying:

![xkcd.com - Python](https://imgs.xkcd.com/comics/python.png)

> Source: [xkcd.com - Python](https://xkcd.com/353/)

In principle, the introduction of modular programming has enabled us to take a
piece of code, regardless of how trivial or complex it is, and use it later,
once we have forgotten how it really works. Then, the introduction of massive
on-line package repositories, such as [PyPI](https://pypi.org) for Python, or
[npm Registry](https://www.npmjs.com/) for JavaScript, took this loaded shotgun
and gave it to random people on the internet, also known as "open-source
community".

Regardless of your opinion, this is how coding is done nowadays, and the ease of
this process commonly separates the most used languages from the obsolete ones.
Which is why I don't really understand, how Python got away with a module
management system, that is way more confusing that it should be.

## The easy way: Importing published packages

First of all, there is a clear distinction to be made -- while you are
installing **packages** via `pip` or any of its alternatives, what you later
import are actually **modules**. In principle, modules are quite simple, as
module is

1. either a single **text file** with `.py` ending,
2. or a **directory** with a `__init__.py` text file.

A package is then also a module, that you can install in a standard way with a
package manager (for more details, see [Glossary - Python Documentation](
https://docs.python.org/3/glossary.html#term-package)). After installing a
package, it can then be imported:

```sh
$ pip install numpy
...
$ python
Python 3.14.0 ...
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> print(numpy)
<module 'numpy' from '.../site-packages/numpy/__init__.py'>
```

The imported module object contains various other objects, most commonly
functions and classes (either defined in the module file or imported from
elsewhere), as well as other modules (either contained in the module directory
or imported from elsewhere). However, even here, things might start getting a
bit confusing:

```sh
>>> import matplotlib
>>> print(matplotlib.animation)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    print(matplotlib.animation)
          ^^^^^^^^^^^^^^^^^^^^
  File ".../site-packages/matplotlib/_api/__init__.py", line 218, in __getattr__
    raise AttributeError(
        f"module {cls.__module__!r} has no attribute {name!r}")
AttributeError: module 'matplotlib' has no attribute 'animation'

>>> import matplotlib.animation
>>> print(matplotlib.animation)
<module 'matplotlib.animation' from '.../site-packages/matplotlib/animation.py'>
```

As it turns out, not all submodules are equal! Running only the first import
only loads whatever is defined, or more commonly imported, in the
`matplotlib/__init__.py` file. Meanwhile, the `matplotlib/` package might
contain other submodules, which must be imported explicitly.

## The messy way: Relative imports

Now comes the fun part -- you are going to design your own module! Or at least a
directory of multiple script files, because why would one create (often empty)
file without a real purpose. You might have `script1.py` with function `foo`,
and `script2.py` with function `bar`. In `script2.py`, you might be tempted to
do:

```py
from .script1 import foo
```

That one imports the other file, and loads the function `foo`; what could
possibly go wrong? Obviously, you test that:

```sh
$ python script2.py
Traceback (most recent call last):
  File ".../script2.py", line 1, in <module>
    from .script1 import foo
ImportError: attempted relative import with no known parent package

$ python -m script2
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File ".../script2.py", line 1, in <module>
    from .script1 import foo
ImportError: attempted relative import with no known parent package

$ python
>>> import script1
>>> script1.foo
<function foo at 0x7fbc85f53740>
>>> import script2
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    import script2
  File ".../script2.py", line 1, in <module>
    from .script1 import foo
ImportError: attempted relative import with no known parent package
```

Elaborating on the error message, it turns out that this method of importing modules is only permitted within a package. Therefore, we create a directory `package1`, with an empty `__init__.py` to make it a module, and move both scripts there. Now, we can do:

```sh
$ python -m package1.script1
$ python -m package1.script2
$ python ./package1/script1.py
$ python ./package1/script2.py
Traceback (most recent call last):
  File ".../package1/script2.py", line 1, in <module>
    from .script1 import foo
ImportError: attempted relative import with no known parent package
```

The imports work, but only when we run the scripts as modules using the `-m` flag. However, more commonly, relative imports are not used in scripts that we run, but rather inside a module. Therefore, we create a third script `main.py` in the parent directory:

```py
from package1.script1 import bar
from package1.script2 import bar as bar2

print(bar(), bar2())
```

This method of importing already includes the information about the parent
package `package1`, so the relative imports in `script2.py` work as expected.

TL;DR: If you were thinking of using relative imports in your project before reading this, DON'T. It will save you a lot of trouble.

For more details, see [Package Relative Imports - Python Documentation](
https://docs.python.org/3/reference/import.html#package-relative-imports).

## The right way: Absolute imports

After all this mess, there is actually a simple solution to all of this -- just
use absolute imports everywhere. Instead of

```py
from .script1 import foo
```

use

```py
from script1 import foo
```

or, if you are inside a package,

```py
from package1.script1 import foo
```

Obviously, for longer package names or more nested structures, this might get a
bit tedious (which was the original idea behind relative imports). However, the
benefits of absolute imports outweigh the downsides by far -- you will never
have to deal with the confusing relative import errors again, and your code will
be much more readable to others (and your future self).

The only thing to keep in mind is that the module you are importing must be visible by Python, meaning that it is in either:

1. the current working directory,
2. typical directory for installed packages (e.g., `site-packages`),
3. any directory specified in the `PYTHONPATH` environment variable.

You can always check the visible directories by:

```py
import sys
print(sys.path)
```

Note that for packages you intend to distribute, the solution is trivial, as the
package manager will take care of installing the package in the right place. You
can use this for your own projects as well, by installing them in "editable"
mode:

```sh
$ pip install -e ./package1
```

That is, a symbolic link will be created in the `site-packages` directory,
pointing to your local package directory, meaning that all changes are
immediately visible.
