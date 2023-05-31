# Juypter setup 

![juypter image](../images/juypter.png)

This guide is a short guide to set up Juypter on your local machine, this can be done on Windows, Mac and Ubuntu systems without an anaconda setup.

# Basic Setup

Steps: 

1. Download Python on to your machine. Please go to the [download section](https://www.python.org/downloads/) for that latest download, we are presuming you are using Python 3

2. Check that it has installed correctly by typing in `python` into your command terminal and also check `pip` is also installed

3. Run `pip install jupyter` and after the download you can then run `juypter notebook` this will then run up your notebook 

# Virtual Environment Setup (Linux setup)

This is linux setup but only needs to be tweaked slightly to work in other environments

Steps: 

1. Install virtualenv library: `sudo -H pip3 install virtualenv` the `-H` flag ensures that the secuirty policy sets the home environment variable to the home directory of the target user. 

2. Create a directory `mkdir ~/my_project_dir`, cd into that directory `cd ~/my_project_dir` and create the virutal environment `virtualenv my_project_env` 

3. Activate the environment `source my_project_env/bin/activate` then you can just follow step 3 from basic setup to run Juypter notebook.


