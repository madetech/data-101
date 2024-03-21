# Setting up Sphinx in a client environment
This guide has come from the work done with the Skills For Care workstream, and is tailored to assume you are trying to get a working and powerful sphinx configuration within an already existing space. If you are looking to start a project from the beginning, or want to just have a good tutorial for setting up from their documentation, then [check out this link instead](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

## Guide outcomes
- Launch a local Sphinx documentation server that dynamically updates in real time
- Sphinx configuration understands markdown, and all documentation files exist as markdown and not ReStructuredText.
- Sphinx configuration allows dynamic reading of Docstrings, of Google or Numpy standard format.
- How to handle Sphinx within version control.
- Local configuration is ready for potential deployment into CI/CD pipelines with proper routing.
- Demonstrate how to interact with the configuration to change the theme, and use extensions.

# Step-by-step instructions
## Setting up

Source of instructions for this step: https://www.sphinx-doc.org/en/master/usage/installation.html
You can install sphinx via brew, but there are other windows instructions too if you aren't operating on MAC or linux. Note also that I am assuming you do not have an existing folder called "docs" when you run this section.

The guide in the link above instructs to use pip to install sphinx-doc, but sphinx-doc isn't quite right. Also ensure that if you have an environment such as poetry, conda, pipenv, or virtualenv, to make sure you update however you inject depencies into your environment. But here is the pip install command you actually need, which you can translate to your environment.
```bash
pip install -U sphinx
sphinx-build --version
```

Next you can follow the steps here: https://www.sphinx-doc.org/en/master/tutorial/getting-started.html, in particular run the quick start. I have an example of what I used to fill in a project metadata, which you can tailor to your own.

```bash
sphinx-quickstart docs

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
> Separate source and build directories (y/n) [n]: y

The project name will occur in several places in the built documentation.
> Project name: Skills for Care Data Engineering
> Author name(s): Skills for Care
> Project release []: 1.0.0

If the documents are to be written in a language other than English,
you can select a language here by its language code. Sphinx will then
translate text that it generates into that language.

For a list of supported codes, see
https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
> Project language [en]:
```
This creates some files, as you can see in the console output and also as below:
```
Creating file /Users/lukewilliams/git-repositories/sfc-repos/DataEngineering/docs/source/conf.py.
Creating file /Users/lukewilliams/git-repositories/sfc-repos/DataEngineering/docs/source/index.rst.
Creating file /Users/lukewilliams/git-repositories/sfc-repos/DataEngineering/docs/Makefile.
Creating file /Users/lukewilliams/git-repositories/sfc-repos/DataEngineering/docs/make.bat.
```
- conf.py is where you will store most of your configuration for Sphinx that you control
- index.rst is a ReStructuredText file that serves as the landing page for your documentation server - its the first page you will see.
- the 2 variations of the Makefile and make.bat are for Linux/Mac and Windows systems respectively. You are unlikely to make changes to this directly in this guide.


Now you can build it:
`sphinx-build -M html docs/source/ docs/build/`
In the console output, you should see a link generated that you can paste into your browser.
This should open up the index page. 

But ReStructuredText is not how a lot of projects store content like READMEs. They normally use markdown, and exist outside of this new space that we have created.

In the section we will look to address both of these issues.

## Adding Markdown support to your Sphinx Configuration

Sources for this section: 
- https://www.sphinx-doc.org/en/master/usage/markdown.html#configuration
- https://www.sphinx-doc.org/en/master/tutorial/first-steps.html
- https://myst-parser.readthedocs.io/en/latest/syntax/optional.html

The first thing you will need to do is configure Sphinx to understand Markdown. This will involve another dependency and a change to the conf.py file we generated earlier.
Run this command (and update any environment dependencies you might have)
`pip install --upgrade myst-parser`

And then add an extensions section to the conf.py if one doesn't exist, that looks like this:
`extensions = ["myst_parser"]`
Note the documentation for this parser is here, [extending the regular markdown functionality to work with Sphinx](https://myst-parser.readthedocs.io/en/latest/syntax/optional.html)

### Manually change the index.rst to index.md

Curious if its possible to start the entire project in markdown rather than rst? Well you can! Your index.rst file no longer needs to be ReStructuredText. At this stage, if that is the only file you need to change into a markdown format file then you can make this change manually without introducing additional dependencies.

To do this, and learn from the documentation, the main difference between the .rst and .md formats for your documentation is how the syntax of the dynamic functionality looks. Take the following example which should describe how to convert from the index.rst to index.md:

Example: RST
```
Welcome to Lumache's documentation!
===================================

.. toctree::
    :maxdepth: 2
    :caption: Contents:


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

Example: MD. Note that I can't use triple tick notation to denote what to put for markdown within this format, in a markdown file, so I've used '\' escape characters, but in your own script you must not use these!
```
# Welcome to Lumache's documentation!

\`\`\` toctree::
:caption: Contents:
:maxdepth: 2
\`\`\`

# Indices and tables
- {ref}`genindex`
- {ref}`modindex`
- {ref}`search`
```

### Converting all .rst files to markdown in one go
IF you have multiple .rst files you want to change, there is a parser for this, and it's usage is demo'd in this [youtube video](https://youtu.be/qRSb299awB0?feature=shared&t=1189). The short form of the steps is here:
`pip install rst-to-myst[sphinx]`

Which you can then use:
`rst2myst convert docs/**/*.rst`

## Referencing an existing root README file
Do you have a directory structure a bit like this, and want to reference README?
|- src/
|- docs/
|-- build/
|-- source/
|- README.md

Fortunately the [video](https://www.youtube.com/watch?v=qRSb299awB0) showed how to do this too. It makes use of **directives**, which I think is the name given to these triple backtick blocks

With this, we can use the {include} directive and then just use the README that is 2 directories above, i.e.
```{include} ../../README.md
:relative-images:
```

Adding this block to your index.md file you should render your existing README.md now in your new space.