---
title: "Community Practices"
teaching: 0
exercises: 75
questions:
- "Why should I follow community standards?"
- "What community standards exist for Python and C++?"
- "How can I make sure I'm following these standards?"
- "How should I document my projects?"
objectives:
- "Understand the benefits of following community conventions and standards"
- "Use existing tools to validate that your code conforms to community standards"
- "Learn how to make software projects shareable under an Open Source license"
- "Use existing tools to turn docstrings into documentation"
keypoints:
- "Community standards make it easier for developers to join an existing project"
- "Linters are tools used to ensure code conforms to community standards"
---

## Community Practices

See the [welcome video](https://youtu.be/RuXNrXiXv98) and matching [slides](../slides/3.2-Community-Practices-and-Refactoring.pptx) with per-slide notes.

### Docstrings Again

Why do we write docstrings in Python?
Well, in addition to the benefits they give us of having the documentation right next to the code, there's some useful tooling that we can use.

One of the nice features of Python is the integrated help available for almost every component.
We can see this help text using the `help` function from a Python interpreter.

~~~
help(list)
~~~
{: .language-python}

~~~
Help on class list in module builtins:

class list(object)
 |  list(iterable=(), /)
 |
 |  Built-in mutable sequence.
 |
 |  If no argument is given, the constructor creates a new empty list.
 |  The argument must be an iterable if specified.
 |
 |  Methods defined here:
 |
 |  __add__(self, value, /)
 |      Return self+value.
 |
 |  __contains__(self, key, /)
 |      Return key in self.
 |
 |  __delitem__(self, key, /)
 |      Delete self[key].
 |
 |  __eq__(self, value, /)
 |      Return self==value.
 ...
~~~
{: .output}

This help text should be available for every object in the Python standard library (every function, class, etc. not keywords like `for` and `if`).
By adding docstrings to our functions and classes, we can get the same functionality for our own code.

~~~
def fibonacci(n):
    """Calculate the Fibonacci number of the given integer.

    A recursive implementation of Fibonacci.

    :param n: integer
    :raises ValueError: raised if n is less than zero
    :returns: Nth Fibonacci number
    """
    if n < 0:
        raise ValueError('Fibonacci is not defined for N < 0')
    if n == 0:
        return 0
    if n == 1:
        return 1

    return fibonacci(n - 1) + fibonacci(n - 2)
~~~
{: .language-python}

~~~
help(fibonacci)
~~~
{: .language-python}

### Building Documentation with Sphinx

One of the most important things you can do to make it easier for others to use or extend your code is to provide documentation.
We've just seen docstrings, but wouldn't it be great if you could take the docstrings you've written and turn it into documentation that you can publish online with your code?

Sphinx lets us do exactly that, but it is a little tricky to get set up properly.
First we need to activate our virtual environment and install Sphinx, then we create a new directory `docs` in which to put our documentation.
Finally, we use a Sphinx quickstart tool to create the documentation structure for us.

~~~
cd code
source venv/bin/activate
pip3 install sphinx
mkdir docs
cd docs
sphinx-quickstart
~~~
{: .language-bash}

You will then be asked to provide some information about the project we're trying to document.
For each question, if there's a default value it will be shown in square brackets.

~~~
Welcome to the Sphinx 3.2.1 quickstart utility.

Please enter values for the following settings (just press Enter to
accept a default value, if one is given in brackets).

Selected root path: .

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
> Separate source and build directories (y/n) [n]: y

The project name will occur in several places in the built documentation.
> Project name: SABS Project
> Author name(s): James Graham
> Project release []: 0.1.0

If the documents are to be written in a language other than English,
you can select a language here by its language code. Sphinx will then
translate text that it generates into that language.

For a list of supported codes, see
https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
> Project language [en]:

Creating file /home/jag1e17/Documents/swc/2020-software-engineering-day3/code/docs/source/conf.py.
Creating file /home/jag1e17/Documents/swc/2020-software-engineering-day3/code/docs/source/index.rst.
Creating file /home/jag1e17/Documents/swc/2020-software-engineering-day3/code/docs/Makefile.
Creating file /home/jag1e17/Documents/swc/2020-software-engineering-day3/code/docs/make.bat.

Finished: An initial directory structure has been created.

You should now populate your master file /home/jag1e17/Documents/swc/2020-software-engineering-day3/code/docs/source/index.rst and create other documentation
source files. Use the Makefile to build the docs, like so:
   make builder
where "builder" is one of the supported builders, e.g. html, latex or linkcheck.
~~~
{: .output}

The Sphinx quickstart tool provides us with sensible default values, but we need to do a little bit of customisation to `config.py`.
Open the file `code/docs/source/config.py` and make these changes to the path and add autodoc to the list of extensions:

~~~
# Configuration file for the Sphinx documentation builder.
#
# This file only contains a selection of the most common options. For a full
# list see the documentation:
# https://www.sphinx-doc.org/en/master/usage/configuration.html

# -- Path setup --------------------------------------------------------------

# If extensions (or modules to document with autodoc) are in another directory,
# add these directories to sys.path here. If the directory is relative to the
# documentation root, use os.path.abspath to make it absolute, like shown here.
#
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))


# -- Project information -----------------------------------------------------

project = 'SABS Project'
copyright = '2020, James Graham'
author = 'James Graham'

# The full version, including alpha/beta/rc tags
release = '0.1.0'


# -- General configuration ---------------------------------------------------

# Add any Sphinx extension module names here, as strings. They can be
# extensions coming with Sphinx (named 'sphinx.ext.*') or your custom
# ones.
extensions = [
    'sphinx.ext.autodoc',
]

# Add any paths that contain templates here, relative to this directory.
templates_path = ['_templates']

# List of patterns, relative to source directory, that match files and
# directories to ignore when looking for source files.
# This pattern also affects html_static_path and html_extra_path.
exclude_patterns = []


# -- Options for HTML output -------------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
#
html_theme = 'alabaster'

# Add any paths that contain custom static files (such as style sheets) here,
# relative to this directory. They are copied after the builtin static files,
# so a file named "default.css" will overwrite the builtin "default.css".
html_static_path = ['_static']
~~~
{: .language-python}

Once we've made these changes to the configuration, we need to tell Sphinx which Python files we want it to get docstrings from.
In practice we would create a new documentation page for each module, but for the sake of simplicity, we'll add our module to the main page of the documentation for now.
To the file `code/docs/source/index.rst` we need to add an `automodule` block to document the file `academic.py`:

~~~
.. SABS Project documentation master file, created by
   sphinx-quickstart on Mon Oct 12 16:02:55 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to SABS Project's documentation!
========================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

.. automodule:: academic
   :members:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
~~~
{: .source}

Documentation on this part of Sphinx is available at [this link](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html).

Now, we should be ready to build our documentation:

~~~
make html
~~~
{: .language-bash}

To view our documentation, we can make use of another bit of built in Python functionality.
The Python standard library includes a basic webserver, that's often useful for sharing small files and looking at things in your browser.
This isn't suitable for use in production, but is good for testing in cases like this.

To run this webserver:

~~~
python -m http.server -d build/html
~~~
{: .language-bash}

This tells Python to run the module `http.server` with the server running from the `build/html` directory.
Then in a web browser navigate to `http://localhost:8000` and you should see your documentation page.

If you used your existing code from yesterday for the previous docstring exercises then this page might not have any documentation for the classes in the file `academics.py`
If this is the case, then copy across your docstrings into the reference version of `academic.py` in today's `code` directory.

When writing documentation for a real project, there are a few useful websites such as [ReadTheDocs](https://readthedocs.org/) which allow us to host our documentation online for free for open source projects.

> ## More Documentation
>
> Try adding documentation for some of the other files in the `code` directory, or code we have written previously.
> Can you repeat these steps to add these files to our Sphinx documentation?
{: .challenge}

### Style Guides & Linting

As we have discussed this morning, one of the most important things we can do to make sure our code is accessible to others is to make sure that it is cleanly formatted and uses sensible, descriptive names.
In order to help us format our code, we generally follow guidelines known as **style guides**.
A style guide is a set of conventions that we agree upon with our colleagues, to ensure that everyone contributing to the same project is producing code which looks similar.

While a group of developers may choose the write and agree upon a new style guide unique to each project, in practice many programming languages or problem domains have a single style guide or set of style guides which is adopted almost universally.
For example in C++, the most popular is probably [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html), but there are also a number of others with popularity in a particular domain.
The [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines) are an extensive reference intended more for the creators of software engineering tools, but may also be worth having a look at.
In Python, although we do have a choice of style guides available, the [**PEP8**](https://www.python.org/dev/peps/pep-0008/) style guide is used in almost all cases.

As you may have noticed by now, one of the advantages of using a proper code editor like VSCode is that we get recommendations for formatting our code.
The recommendations made by VSCode and and many other editors are mostly taken from the PEP8 style guide.

There are also tools, known as **linters**, whic exist to validate your code against particular community standards, but two of the main ones to be aware of for Python are:
- `flake8` (actually a combination of `pyflakes` and `pycodestyle`)
- `pylint`

We'll also have a look at `cpplint` for C++ which validates your code against the Google C++ style guide.

As well as checking for adherence to a style guide, many linters also attempt to identify suspicious parts of the code which may indicate an error has been made.
One example of this is checking that there are no unused or undefined variables - an unused or undefined variable may be the result of a typo.
Because of this, the process of linting is particularly important for languages which are not typically compiled, such as Python.
Some of the benefits of linting, such as checking for undefined variables, happen at compile time when using a compiled language such as C++, but some of the other checks are still useful when using compiled languages.

Since the Python linters `flake8` and `pylint` are not part of the standard Python distribution, we're going to have to install them ourselves.
The C++ linter `cpplint` is actually written in Python, so we'll install that as well.
So, using `pip` again:

~~~
cd code
source venv/bin/activate
pip install flake8 pylint cpplint
~~~
{: .language-bash}

We can run these tools over our code from the command line:

~~~
flake8 academic.py
~~~
{: .language-bash}

But we can also integrate them with our editor.
In VSCode, if we press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and type `linter`, we get a list of linters that VSCode can work with.
From this list, select `flake8`, and now every time we save our code, we should get highlights where Flake8 produces a warning.

If VSCode says it can't find Flake8, it might be because it's not detecting our virtual environment properly.
In this case, press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> again and this time type `interpreter`.
From the 'Python: Select Interpreter' menu we can tell VSCode which interpreter to use by selecting one of the options, or navigating to `code/venv/bin/python3` with the file explorer.

We'll come back to linting this afternoon in an exercise on refactoring...

### Licensing

Software licensing can be a whole topic in itself, so we'll just summarise here.
Your institution's Intellectual Property (IP) team will be able to offer specific guidance that fits the way your institution thinks about software.

In IP law, software is considered a creative work of literature, so any code you write automatically has copyright protection applied.
This copyright will usually belong to the institution that employs you, but this may be different for PhD students.
If you need to check, this should be included in your employment / studentship contract or talk to your university's IP team.

Since software is automatically under copyright, without a license no one may:
- copy it
- distribute it
- modify it
- extend it
- use it (unclear - this has not been properly tested in court yet)

Fundamentally there are two kinds of license, **Open Source** licenses and **Proprietary** licenses, which serve slightly different purposes.

Proprietary licenses are designed to pass on limited rights to end users, and are most suitable if you want to commercialise your software.
They tend to be customised to suit the requirements of the software and the institution to which it belongs - again your institutions IP team will be able to help here.

Open Source licenses are designed more to protect the rights of end users - they specifically grant permission to make modifications and redistribute the software to others.
The website [Choose A License](https://choosealicense.com/) provides recommendations and a simple summary of some of the most common open source licenses.

Within the open source licenses, there are two categories, **copyleft** and **permissive**.
The permissive licenses such as [MIT](https://choosealicense.com/licenses/mit/) and the multiple variants of the BSD license are designed to give maximum freedom to the end users of software.
These licenses allow the end user to do almost anything with the source code.

The copyleft licences in the [GPL](https://choosealicense.com/licenses/gpl-3.0/) still give a lot of freedom to the end users, but any code that they write based on GPLed code must also be licensed under the same license.
This gives the developer assurance that anyone building on their code is also contributing back to the community.
It's actually a little more complicated than this, and the variants all have slightly different conditions and applicability, but this is the core of the license.

Which of these types of license you prefer is up to you and those you develop code with.


### Version Control

We should mention version control here, as this is a very important aspect of collaborative software development - but we won't cover it properly, since that's coming up later in the course.
Version control allows you to track the history of all changes to your software - including being able to manage multiple people collaborating on the same files.
With each change we provide a brief description, to help other developers (or ourselves in the future) understand the purpose of the change.

Using a site like GitHub (there are a few major alternatives, like GitLab or BitBucket) gives us access to a range of project management tools such as issue trackers and project boards.
Good use of these tools can make it much easier to develop software collaboratively.

That's enough for now, we'll be seeing much more about it soon.


> ## Action Stations (Again)!
> This time we're going to look at a real software project which is an example of good practices.
>
> Imagine that your collaborators have now asked you to contribute to one of their projects.
> They've sent you a link to the repository in GitHub and want you to familiarise yourself with the code before you begin making your changes.
>
> Again in small groups, look at the repository for the [Requests library](https://github.com/psf/requests).
> Examine the contents and structure of the repository (code, documentation, project management, etc.) and as a group discuss what has been done to make it easier to make use of this project, both as a user and as a developer wanting to contribute.
> Make a list of things which have been done to make it easier for people to use, install, maintain, extend and generally collaborate, with this software.
> Is there anything missing that you would find useful?
> Is there anything the project does that you think is bad? (no project is all good)
>
> Make notes on your findings, and then briefly report back the top three things to the class.
> For a maximum of three minutes, for each of the three things you found (i.e. a minute each), report back the following:
>
> 1. A few sentences description of what you've found
> 2. A few sentences on how this benefits users of developers of the software
{: .challenge}

{% include links.md %}
