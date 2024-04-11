# Setting up Sphinx in a client environment
This guide has come from the work done with the Skills For Care workstream, and is tailored to assume you are trying to get a working and powerful sphinx configuration within an already existing python space. If you are looking to start a project from the beginning, or want to just have a good tutorial for setting up from their documentation, then [check out this link instead](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

## Guide outcomes
- What Sphinx even is
- Launch a local Sphinx documentation server that dynamically updates in real time
- Sphinx configuration understands markdown, and all documentation files exist as markdown and not ReStructuredText.
- Sphinx configuration allows dynamic reading of Docstrings, of Google or Numpy standard format.
- How to handle Sphinx within version control.
- Local configuration is ready for potential deployment into CI/CD pipelines with proper routing.
- Demonstrate how to interact with the configuration to change the theme, and use extensions.
- To understand potential use cases of this setup, as docstrings aren't for everyone!

# Introduction - What is Sphinx?

At it's core [Sphinx](https://www.sphinx-doc.org/en/master/index.html) is a documentation generator that uses reStructuredText or Markdown to create static documentation websites. It makes it easy to create intelligent and beautiful documentation for not just python projects, and can be embedded in many existing project setups.
[Sphinx](https://www.sphinx-doc.org/en/master/index.html) can look quite intimidating to a first-time user - it is packed full of features and has a large community base with plugins and themes galore

I hosted a Data CoP that talked about `Sphinx`, and gave some examples of potential alternatives and competitors. If you want to see this CoP you can find the recording information in our [Facilitators sheet](https://docs.google.com/spreadsheets/d/1C4n1miK6Xaa5CHkZmjSQyBRtSm15PyyUpoCk_SzM0NE/edit?usp=sharing).

## When might you want to use Sphinx?
This section will give an insight into when it might be used, and also what other tools exist in this realm.

### Examples of potential use cases
Docstrings are not everyone's cup of tea. **But you also don't need Docstrings to use Sphinx**. Sphinx has some dynamic powers to reduce the amount of time you spend writing documentation that surrounds docstrings, sure, but since it is built off of Markdown in a `docs` directory, it can also set the stage for holding documentation in READMEs close to your code and thus within the same version control when you come to making PRs for code changes.

Here are some potential project scenarios where Sphinx might be a good idea:
- The [Skills for Care workstream](https://github.com/NMDSdevopsServiceAdm/DataEngineering/)
- When there is a project whose non technical stakeholders want better visibility of what the code is doing.
- There is a need for documentation to see which code is responsible for specific parts of the pipeline, and this is particularly relevant if there are ML components to the pipeline stored in the same part of the code base.
- You have multiple components to your project, such as an API front end as well as a webscreen that you want to signpost people toward when they visit your repo.
- You don't have a front end development team and want easy rendering of static web-page documentation for any aspect of your project.
- You have graphics or visuals like C4 diagrams close to your code you want to showcase somewhere.
- You are on a workstream where there is some to no documentation, and you want to incentivise better documentation practises. You can use Sphinx to encourage docstring usage or even better functional practises if there is documentation that can build from it - i.e. do not repeat yourself practises translating across into docs.

### Main counter examples
You might not need to use Sphinx specifically if any of the following apply:
- You are building an API, `Swagger` might be a better use case for this
- The team you are in has good functionally descriptive functions that are self explanatory without docstrings
- There is no need for code level documentation
- You have an already established documentation server like Confluence that is well established for updating during PRs.

### Other Competitors

#### Similar - Swagger

The most significant difference is that Swagger UI is not general enough, it is primarily focused on documenting RESTful APIs. While it can be used with projects in different programming languages, it is specifically designed for documenting the API endpoints and their associated operations. Sphinx is a tool that can be used to document projects in various programming languages, including Python, JavaScript, C++, and more. It has built-in support for documenting different types of projects, including APIs, libraries, and command-line tools.
Where it lacks in interactivity that swagger might have, it makes up for in flexibility and design of content, without having to have a front-end development team, and if there is time and interest at the end we can go through more differences between them, but this is enough of a difference for now.

Swagger versus Sphinx: https://stackshare.io/stackups/sphinx-vs-swagger-ui - more breakdowns

#### Other documentation generators
All of the tools in this section are other alternatives to Sphinx, with specific pros and cons.

**pdoc** - Its code is a fraction of Sphinx’s complexity and the output is not quite as polished, but it works with zero configuration in a single step. It also supports docstrings for variables through source code parsing. Otherwise it uses introspection (same as Sphinx). Worth checking out if Sphinx is too complicated for your use case and you only have python code, as currently the only way to have this include markdown is by using reStructuredText's `.. include::` Directive, defeating the point of only using markdown in a project.

**pydoctor** - A successor to the popular epydoc, it works only for Python 2. Main benefit is that it traces inheritances particularly well, even for multiple interfaces. Works on static source and can pass resulting object model to Sphinx if you prefer its output style. Pydoctor has a clean look to it, but lacks themes like Sphinx has, more on this later.

**doxygen** - Not Python-exclusive and its interface can be a bit crowded and ugly. It claims to be able to generate some documentation (mostly inheritances and dependencies) from undocumented source code, perhaps giving it use in really large established projects without documentation. It does appear to have dynamic diagram support of class hierarchies, so perhaps a better consideration for dev heavy projects, and teams familiar with old school C++ might already know this tool from its wide use in multiple languages - however Sphinx also has an extension to enabled Graphviz diagrams straight from source from what I've seen which possibly offsets this advantage some what... again that increased flexibility in design.

Honourable mention:
You might also have heard about [readthedocs](https://about.readthedocs.com/?ref=readthedocs.com) but this is not free and open source, but can be used to host Sphinx enabled projects, and even has it's own Readthedocs theme within Sphinx. However Sphinx can be deployed in github pages, and there is instructions for this on our Data-101 page guide for Sphinx

# Step-by-step instructions for setting up a Powerful Sphinx configuration
## Setting up

Source of instructions for this step: https://www.sphinx-doc.org/en/master/usage/installation.html
You can install Sphinx via brew, but there are other windows instructions too if you aren't operating on MAC or linux. Note also that I am assuming you do not have an existing folder called "docs" when you run this section.

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

Congrats - you've completed the basic tutorial sphinx provides at this point. 

The first thing you might notice though is that after this basic setup there is a *lot of noise in version control*. So in the next section let us level up our deployment, and firstly deal with that:

## Cleaning up version control

Simply add `docs/build` somewhere in your `.gitignore`. 
What you are doing here is telling git to ignore all of the HTML, CSS and those sort of rendered files that are created whenever you run `sphinx-build` or an equivalent. These will regenerate whenever you make changes, and you COULD commit them to version control if you want to, but you don't NEED to. What controls these files is your configuration and files within `docs/source`, and that is more valuable.

## Adding Markdown support to your Sphinx Configuration

But ReStructuredText is not how a lot of projects store content like READMEs. They normally use markdown, and exist outside of this new space that we have created.

In the section we will look to address both of these issues.

Sources for this section: 
- https://www.sphinx-doc.org/en/master/usage/markdown.html#configuration
- https://www.sphinx-doc.org/en/master/tutorial/first-steps.html
- https://myst-parser.readthedocs.io/en/latest/syntax/optional.html

The next thing you will need to do is configure Sphinx to understand Markdown. This will involve another dependency and a change to the conf.py file we generated earlier.
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
Do you have a directory structure a bit like this, and want to reference an existing README?
|- src/
|- docs/
|-- build/
|-- source/
|--- index.md
|- README.md

Fortunately the [video](https://www.youtube.com/watch?v=qRSb299awB0) showed how to do this too. It makes use of **directives**, which I think is the name given to these triple backtick blocks

With this, we can use the {include} directive and then just use the README that is 2 directories above, i.e.
```{include} ../../README.md
:relative-images:
```

Adding this block to your index.md file you should render your existing README.md now in your new space.

# Powering up your basic sphinx setup
By this point you now have a basic working Sphinx configuration with Markdown support. But we can take Sphinx further than this, and we will explore some more powerful functionality in this section.

## Configuring autofunctions and automodules
One of the core foci of the Data CoP talk was the idea that Sphinx can dynamically read function docstrings from your code repository and thus offer a way of expanding the documentation around the code, whilst only making changes in the code. Setup for this isn't obvious following the tutorials, so here I will describe how to get this working with code not just in the new docs directory, but also existing code that exists elsewhere.

### Steps to automatic docs
1. Enable the `"sphinx.ext.autodoc"` extension in `conf.py`
2. Extend the path in the conf.py. This step is important and was the primary cause of problems in this realm when referencing code external to the `docs` directory. See more below
3. Simply reference the code function you want to document, see below for instructions

| Extending the path |
| ----------------- |
| At it's heart, `conf.py` is a python file, and so you can import base packages and this is understood by Sphinx. <br>We can leverage this to extend the path and thus allow sphinx to find code elsewhere in the project:
```
import sys, os

sys.path.insert(0, os.path.abspath("../.."))
``` 
Put this anywhere in the `conf.py` file and you will find that imports should work across the project|

| Syntax for autofunctions and automodules |
| ---------------------------------------- |
| Remember that you need `{eval-rst}` after the first triple backtick in each case, I just can't do this in Markdown in a tutorial about it because it's a magic function. |
|<br>
```
.. autofunction:: utils.utils.remove_already_cleaned_data

.. automodule:: jobs.clean_ascwds_workplace_data
   :members:
```
<br>

| Correcting a Docstring and re-rendering it |
| ------------------------------------------ |
| I noticed that making a change to a docstring didn't automatically update it in any auto-doc reference. Take the following example, imagine being contained within an eval-rst colon fence:<br><br>`autofunction:: utils.utils.latest_datefield_for_grouping`<br><br>The main steps I tried that seemed to work, revolved around the followingg steps:<br><br>1. Closing the autobuild server down<br>    <br>2. Deleting that autofunction entirely, saving the docs<br>    <br>3. Re-running autobuild<br>    <br>4. THEN put the autofunction back in re-save.<br>    <br>5. This seemed to eliminate the cache of the old variant and load the new one, and if not, try closing the browser too and repeating the steps above, then to re-open the browser.<br>    <br><br>It is a little fiddly with the autodocs! |

## Some other level up options

| Setting up an autobuilder |
| ------------------------- |
| This is quite a simple but powerful step especially for locally testing changes to documentation. What this step will do is allow you instead of running `sphinx-build` to instead run a new command, `sphinx-autobuild`, with the same options passed in as before. The difference is that now when you make a change to a markdown file and save it, **it will update in real time** |
| To set this up, you will need another dependency to install in the usual ways and then via pip: <br><br> `pip install sphinx-autobuild` <br><br> And that's it. Now you can run `sphinx-autobuild docs/source/ docs/build/` and spin up a local server as before, but that will rebuild whenever you make changes to a Markdown file and save it. There are some caveats to this which we will encounter later.|

| Setting a theme |
| --------------- |
| The basic template of Sphinx, Alabaster, is a bit lame, and you can find some other templates in the [sphinx theme gallery](https://sphinx-themes.org/). <br> Here I'm going to very quickly run through how to make this better with an example. Simply, you need to change the theme in the `conf.py` configuration and install the python package (and if you have something like a pipfile don't forget to update that too):  <br> `html_theme = "furo"`<br><br>`pip install furo`<br>  <br>The theme is more modern than the default, and looks way better. It also allows for dark/light screen switching for one thing, and you can find more of it's features on the [furo github page](https://github.com/pradyunsg/furo "https://github.com/pradyunsg/furo").<br><br>More themes in their [Themes Gallery](https://sphinx-themes.org/ "https://sphinx-themes.org/") |

| Sphinx duration |
| --------------- |
| This extension shows how long it takes to render the pages you build in your documentation. This time appears in the console whenever you make a change and it re-builds the page, and could be useful for page debugging or spotting larger build times, but is otherwise fairly niche. <br> For this you will need to add: <br>`"sphinx.ext.duration"`  <br> to your extensions |

| Allow links to subheadings |
| -------------------------- |
| You need another internal extension, `"sphinx.ext.autosectionlabel"`<br><br>This allows you to reference a subheading by doing something like the following:<br><br>`` {ref}`Allow links to subheadings` `` |

| Google Docstring support |
| ------------------------ |
| For this we need to add the built in extension to the `conf.py` file as so: <br><br>`"sphinx.ext.napoleon",`<br><br>This adds support for reading google docstrings of the format as described below, and found on this [link](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html#google-vs-numpy "https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html#google-vs-numpy") |

By this point, and building the project a couple of times, you might have quickly noticed a lot of noise in version control. This comes from all of the files that get built when running the `sphinx-build` commands.


