# Foreword
The following guide takes you through the provisioning of your laptop assuming you are using macOS or Linux, for a windows guide to setting up your device with Python please try [here](https://docs.python.org/3/using/windows.html).

# Tutorial: Setting up your Python environment
When working on a Python project it is typically beneficial to use virtual environments. This massively improves collaboration as it ensures that all developers are working from the same starting point and are able to spin up the project quickly. A virtual Python environment segregates project specific packages from your global installed packages.

This guide will cover how best to install Python and set up a virtual environment for your project. 

# Homebrew
Before going any further (and assuming now that you are working on macOS or Linux), it is advisable to familiarise yourself with [Homebrew](https://brew.sh). Homebrew is a package management system that instals packages to their own directory and then symlinks their files into `/usr/local`. Homebrew won’t install files outside its prefix and you can place a Homebrew installation wherever you like. It is an incredibly powerful and widely used tool. 
To install Homebrew, paste the following command into your terminal or shell prompt:
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

# Installing Python
Both macOS and Linux ship with [Python](https://www.python.org) already installed. However, given some system tools rely on this, it is not advisable to mess with these. Instead we should install our own version(s), upon which we can build our projects. This document will cover two methods for installing Python versions, Homebrew and Pyenv. 


### Homebrew Python
Hopefully you will already be familiar with Homebrew (see above) and already have this set up. When installing Python with homebrew Python 3 is the default version. If you have a specific requirement for a version of Python then you can also specify this, but there will be a better way of managing python versions in a later section. 

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
*Note: If you are using Python 3 then then you will need to use `python3` and `pip3` by default. This can be changed by updating your `alias`. More information can be found [here](https://osxdaily.com/2022/02/15/make-python-3-default-macos/).*

### Pyenv and pyenv-virtualenv
[Pyenv](https://github.com/pyenv/pyenv) is a Python version manager that allows you to install and manage different versions of Python.
One of the powerful offerings of Pyenv for managing multiple local and quick setups is the ability to create different python environments for different python versions and easily activate them anywhere in your file system. This requires another brew dependency called [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

There are instructions for setting this up in the links above, but for simplicity we will combine the resources into the steps below.

1. First we can install them both using `brew`.

```
$ brew update
$ brew install pyenv
$ brew install pyenv-virtualenv
```

2. Next you need to follow the post-installation steps for pyenv and then for pyenv-virtualenv which should be output in the terminal, and can be found on their [github page#set-up-your-shell-environment-for-pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#set-up-your-shell-environment-for-pyenv). *You will likely be using a `bash_profile`* unless you've configured a `zsh` environment, so take a look at the relevant instructions. When combined they will look something like the example below:

For Zsh:

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

source ~/.zshrc
```

Congratulations, you will now have pyenv successfully installed with its pyenv-virtualenv component!
Now you can use `Pyenv` to install specific Python versions and set your global version, as well as build virtual environments on a specific python version. The former of which is explained below:

#### List available Python versions
```
$ pyenv install --list
```

#### List installed versions
```
$ pyenv versions
```

#### Install desired version
```
$ pyenv install <version_number>
```

#### Specify global Python version
```
$ pyenv global <version_number>
```

# Creating a virtual environment

Virtual environments are an essential tool in development. They ensure that developers are all working from the same point and makes setting up projects far easier. Virtual environments are created on top of an existing Python installation, the “base” Python image, and may optionally be isolated from the packages in the base environment, so only those explicitly installed in the virtual environment are available. 

## Using pyenv and virtual-env
As explained in [using pyenv virtualenv with pyenv](https://github.com/pyenv/pyenv-virtualenv?tab=readme-ov-file#using-pyenv-virtualenv-with-pyenv), you can easily create a virtual environment for a specific python version using pyenv and virtualenv. As a simple example, provided you have followed the earlier setup steps, if you want a virtual environment called `my-virtual-env-2.7.10` on python `2.7.10`:

```
pyenv virtualenv 2.7.10 my-virtual-env-2.7.10
```

### Python version problems? How to test your installation!
1. Check your python versions under pyenv with `pyenv versions`, 
2. create a pyenv virtual environment with a different version of python to the pyenv version you are currently on, and try to switch to it.
3. IF this continues to display the old python version you are on, then your `pyenv` isn't swapping your virtual environment as intended.
These steps summarise the above:
```
pyenv install 3.11.9 (if you don't have it)
pyenv virtualenv 3.11.9 test_environment
pyenv activate test_environment
which python
```
That last command should tell you that you are on python version `3.11.9`

### Debugging pyenv environment problems
If the above test failed, you may have outdated commands saved in your `zshrc` or `bash_profile`. They have updated their installation instructions [on their website](https://github.com/pyenv/pyenv?tab=readme-ov-file#set-up-your-shell-environment-for-pyenv)

*Check that your commands are the same as theirs!*


## Using venv
Since the v3.3 release Python has a built in `venv` module [link](https://docs.python.org/3/library/venv.html). This is a lightweight virtual environment tool and is very simple to use for singular, one-off environments. 

To set up a virtual environment first navigate to the root of your project. From here run:
```
$ python3 -m venv <env_name>
```
This will create a folder that specifies the base image as well as any modules, packages or libraries you install. A good naming suggestion is `venv` or `.venv` if you want to hide this from view in finder (although VS Code will typically still be able to see it). Whatever you decide to name your environment, make sure to add the folder to your `.gitignore` file, so you will not upload this to the repo with your next push. 

To activate this environment, that is to work from the virtual base image, run the following:
```
$ source <env_name>/bin/activate
```
If this process has been followed correctly, your terminal should now show the environment name at the beginning of any line. for example:
```
$ (<env_name>) Documents/Development/MadeTech/<your_project>
```
Now, when you install something using pip, the package will instead be installed in the `<env_name>/` folder and not to your system's base environment. 

To leave the virtual environment, run:
```
$ deactivate
```

## Using Virtualenv
The `virtualenv` library is a more feature rich version of the `venv` option discussed above. Among other benefits, it is faster and allows you to specify your base image of Python to be different from your global system version. For a full list of its benefits over `venv`, please read the documentation found [here](https://virtualenv.pypa.io/en/stable/index.html).

>Please note that there is a version of this module that interacts directly with `pyenv` for a full python version and environment management setup. You can find this in the earlier section, so only proceed if you intend to install Virtualenv as a standalone with a specific python version you want to control yourself.

To use `virtualenv`, first install it to you base Python image:
```
$ pip3 install virtualenv
```
From this point, the main difference in use is the command you use to create the virtual environment. 

Navigate to you projects root directory and run:
```
$ python3 -m virtualenv <env_name>
```
To spin up the environment:
```
$ source <env_name>/bin/activate
```
Again, you will be able to see the environment is running because the name of the environment will appear at the start of your command line:
```
$ (<env_name>) Documents/Development/MadeTech/<your_project>
```
To deactivate:

```
$ deactivate
``` 

Once activated any python commands run in that session will only affect the virtual environment such as package instals and updates. 

## Pipenv & Poetry

As well as the above ways to create virtual environments there are two other tools that are popular in setting up python environments at MadeTech: 

[Pipenv](https://pypi.org/project/pipenv/)

[Poetry](https://python-poetry.org/)

Poetry in particular has gained popularity due to being able to spin up clean encapsulated environments, easy packaging and quality checking.


# Requirements 
Now you are able to create, spin up and deactivate virtual environments, you can take advantage of the `requirements.txt` file found with most Python projects. This file typically lists all the packages required to run the project. 

*The following commands should be performed within your virtual environment, otherwise you will be installing packages to your system version of Python.*
### To install the requirements
```
$ pip3 install -r requirements.txt
```
*Note: This assumes the requirements are held in a file called requirements.txt*

### Uninstall the requirements
This is typically not often necessary, however can be useful if you are looking to start fresh:
```
$ pip3 uninstall -r requirements.txt -y
```

### Create a requirements file
Once you are happy with the packages your project needs, or to update the file with any you may have added yourself, run the following command:
```
$ pip3 freeze -> requirements.txt
```

# Installing PySpark
To install PySpark you will first need to install java. We have found jdk@8 most compatible with spark.

```
$ brew install openjdk@8
```
More information can be found [here](https://spark.apache.org/docs/latest/api/python/getting_started/install.html), however you can then simply run the following command in your desired environment:
```
$ pip3 install pyspark
```
If you are looking to install specific dependencies, you can install as shown below:
```
# Spark SQL
$ pip3 install pyspark[sql]
# pandas API on Spark
$ pip3 install pyspark[pandas_on_spark] plotly  # to plot your data, you can install plotly together.
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

[Virtual environments a primer](https://realpython.com/python-virtual-environments-a-primer/)

[Manage projects with pipenv and pyenv](https://www.rootstrap.com/blog/how-to-manage-your-python-projects-with-pipenv-pyenv/)

[The right and wrong way to set Python 3 as default on a Mac](https://opensource.com/article/19/5/python-3-default-mac)
