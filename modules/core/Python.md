# Tutorial: Setting up your Python environment

## Simple setup:

When working on a python project it is beneficial to use virtual environments to ensure that all developers on the project have the same dependencies and are able to spin up an environment quickly. A virtual python environment segregates project specific packages from your global installed packages.

To manage the python versions on your machine locally then install ‘pyenv’ which is a Python version management system. Using pyenv you can set global and local versions of what Python version you would like to run.

Below I am going to outline how you would run up a virtual environment if you installed using Python3 (if you are using Python 2 then please refer to this documentation: [virtualenv](https://packaging.python.org/en/latest/key_projects/#virtualenv) but mostly it is swapping venv to virtualenv).   

To begin please run:

```shell
pip install virtualenv
```

Then to set up virtual environment:

```shell
python3 -m venv {path to new virtual environment}
```

The Python 3 venv approach has the benefit of forcing you to choose a specific version of the Python 3 interpreter that should be used to create the virtual environment. 

Key features of the project to note is:

bin: files that interact with the virtual environment

include: C headers that compile the Python packages

lib: a copy of the Python version along with a site-packages folder where each dependency is installed

To activate your session run this command on your terminal:

```shell
source env/bin/activate
```
 
To deactivate then just run on the terminal:

```shell
deactivate
``` 

Once activated any python commands run in that session will only affect the virtual environment such as download of packages etc. 

## Pipenv & Poetry

As well as the above ways to create virtual environments there are two other tools that are popular in setting up python environments at MadeTech: 

[Pipenv](https://pypi.org/project/pipenv/)

[Poetry](https://python-poetry.org/)

Poetry in particular has gained popularity due to being able to spin up clean encapsulated environments, easy packaging and quality checking.
Delta Setup

If you are testing pyspark locally then all you need to do in your pipfile install:

```
pyspark = “*“
```

To get the latest version and then import ‘from pyspark.sql import SparkSession’  and then at the beginning of your script put: ‘spark = SparkSession.builder.getOrCreate()’ 

If you are working with Databricks then you may have to work with Delta Tables. Then to run up locally you will need to set your pyspark version to ‘3.1.0’ and install on pipfile: 

```
delta-spark = "*"  
```

And then at the beginning of your script you run: 

```
builder = SparkSession.builder.appName("MyApp").config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension").config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")
 
 spark = configure_spark_with_delta_pip(builder).getOrCreate()
```

Please click [here](https://docs.delta.io/latest/quick-start.html#set-up-project) for more information


Useful links:

[Virutal environments a primer](https://realpython.com/python-virtual-environments-a-primer/)

[Manage projects with pipenv and pyenv](https://www.rootstrap.com/blog/how-to-manage-your-python-projects-with-pipenv-pyenv/)
