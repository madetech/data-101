# Foreword
The following guide take you through the provisioning of your laptop assuming you are using macOS or Linux, for a windows guide to setting up your devise with python please try [here](https://docs.python.org/3/using/windows.html)
# Tutorial: Setting up your Python environment
When working on a python project it is beneficial to use virtual environments. This ensures that all developers on that project are using the same dependencies and are able to spin up an environment quickly. A virtual python environment segregates project specific packages from your global installed packages.

This guide will cover how best to install python and set up a virtual environment for your project. 

# Homebrew
Before going any further (and assuming now that you are working on macOS or Linux), it is advisable to familiarise yourself with [Homebrew](https://brew.sh). Homebrew is a package management system that installs packages to their own directory and then symlinks their files into `/usr/local`. Homebrew won’t install files outside its prefix and you can place a Homebrew installation wherever you like. It is an incredibly powerful and widely used tool. 
To install Homebrew, paste the following command into your terminal or shell prompt:
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

# Installing Python
Both macOS and Linus ship with [Python](https://www.python.org) already installed. However, given some system tools rely on this, it is not advisable to mess with these. Instead we should install our own version(s), upon which we can build our projects. This document will cover two methods for installing Python versions, Homebrew and Pyenv. 


### Homebrew Python
Hopefully you will already be familiar with Homebrew (see above) and already have this set up. When installing Python with homebrew Python 3 is the default version. If you have a specific requirement for a version of Python then you can also specify this. 

#### Installing Python 3
```
$ brew install python
```

#### Specifying Python version
```
$ brew install python@2
```

This will also install [pip](https://pypi.org/project/setuptools/) and its dependency [Setuptools](https://pypi.org/project/setuptools/). 
You can keep these up to date using:
```
$ pip install --upgrade pip
$ pip install --upgrade setuptools
```

### Pyenv
[Pyenv](https://github.com/pyenv/pyenv) is a Python version manager that allows you to install and manage different versions of Python. First we can install this using `brew`.

```
$ brew install pyenv
```
In the future, you can upgrade using:
```
$ brew upgrade pyenv
```

Once installed, you can use `Pyenv` to install specific Python versions and set your global version. 

#### List availible Python versions
```
$ pyenv install --list
```

#### List installed versions
```
$ pyenv versions
```

#### Install desired version
```
$ pyenv install 3.5.2
```

#### Specify global Python version
```
$ pyenv global < version number >
```

# Creating a virtual environment

virtual environments are an essential tall in development. It ensures that developers are all working from the same point and makes setting up projects far easier. A virtual environment is created on top of an existing Python installation, known as the virtual environment’s “base” Python, and may optionally be isolated from the packages in the base environment, so only those explicitly installed in the virtual environment are available. 

## Using venv
Since the v3.3 release Python has a built in `venv` module [link](https://docs.python.org/3/library/venv.html). This is a lightweight virtual environment tool and is very simple to use. 

To set up a virtual environment first navigate to the root of your project. From here run:
```
$ python3 -m venv <env_name>
```
This will create a folder that specifies the base image as well as any modules packages or libraies you install. A good naming suggestion is `venv` or `.venv` if you want to hide this from view in finder (although VS Code will typically still be able to see it). Whatever you decide to name your environment, make sure to add the folder to your `gitignore` file, to makes sure you dont upload this to the repo with your next push. 

To activate this environment, that is to work from the virtual base image, run the following:
```
$ source <env_name>/bin/activate
```
If this process has been followed correctly, your terminal should now show the environment name at the beginning of any line. for example:
```
$ (<env_name>) Documents/Development/MadeTech/<your_project>
```
Now, when you install something using pip, the package will instead be installed in the `<env_name>/` folder and not to your systems base environment. 

To leave the virtual environment, run:
```
$ deactivate
```

## Using Virtualenv
Below is outlined how to run up a virtual environment if you installed using Python3 (if you are using Python 2 then please refer to this documentation: [virtualenv](https://packaging.python.org/en/latest/key_projects/#virtualenv) but mostly it is swapping venv to virtualenv).   

To begin run:
```
$ pip install virtualenv
```

Then to set up virtual environment:

```
$ python3 -m venv {path to new virtual environment}
```

The Python 3 venv approach has the benefit of forcing you to choose a specific version of the Python 3 interpreter that should be used to create the virtual environment. 

Key features of the project to note is:

bin: files that interact with the virtual environment

include: C headers that compile the Python packages

lib: a copy of the Python version along with a site-packages folder where each dependency is installed

To activate your session run this command on your terminal:

```
$ source env/bin/activate
```
 
To deactivate then just run on the terminal:

```
$ deactivate
``` 

Once activated any python commands run in that session will only affect the virtual environment such as download of packages etc. 

## Pipenv & Poetry

As well as the above ways to create virtual environments there are two other tools that are popular in setting up python environments at MadeTech: 

[Pipenv](https://pypi.org/project/pipenv/)

[Poetry](https://python-poetry.org/)

Poetry in particular has gained popularity due to being able to spin up clean encapsulated environments, easy packaging and quality checking.


# Requirements 
Now you are able to create, spin up and deactivate virtual environments, you can take advantage of the requirements.txt file found with most Python projects. This file typically lists all the packages required to run the project. 

*The following commands should be performed within your virtual environment, otherwise you will be installing packages to your system version of Python.*
### To install the requirements
```
$ pip install -r requirements.txt
```
*Note: This assumes the requirements are held in a file called requirements.txt*

### Uninstall the requirements
This is typically not often necessary, however can be useful if you are looking to start fresh:
```
$ pip uninstall -r requirements.txt -y
```

### Create a requirements file
Once you are happy with the packages your project needs, or to update the file with any you may have added yourself, run the following command:
```
$ pip freeze -> requirements.txt
```

# Installing PySpark
To install PySpark you will first need to install java. We have found jdk@8 most compatible with spark.

```
$ brew install openjdk@8
```
More information can be found [here](https://spark.apache.org/docs/latest/api/python/getting_started/install.html), however you can then simply run the following command in your desired environment:
```
$ pip install pyspark
```
If you are looking to install specific dependencies, you can install as shown below:
```
# Spark SQL
$ pip install pyspark[sql]
# pandas API on Spark
$ pip install pyspark[pandas_on_spark] plotly  # to plot your data, you can install plotly together.
```

## Delta Tables
If you are working with Databricks then you may have to work with Delta Tables. Then to run up locally you will need to set your PySpark version to ‘3.1.0’ and install on pipfile: 

```
$ delta-spark = "*"  
```

And then at the beginning of your script you run: 

```
$ builder = SparkSession.builder.appName("MyApp").config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension").config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")
 
$ spark = configure_spark_with_delta_pip(builder).getOrCreate()
```

Please click [here](https://docs.delta.io/latest/quick-start.html#set-up-project) for more information


# Useful links:

[Virutal environments a primer](https://realpython.com/python-virtual-environments-a-primer/)

[Manage projects with pipenv and pyenv](https://www.rootstrap.com/blog/how-to-manage-your-python-projects-with-pipenv-pyenv/)

[The right and wrong way to set Python 3 as default on a Mac](https://opensource.com/article/19/5/python-3-default-mac)
