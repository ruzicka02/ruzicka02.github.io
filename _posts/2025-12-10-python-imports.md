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

For more details, see [Package Relative Imports - Python Documentation](
https://docs.python.org/3/reference/import.html#package-relative-imports).