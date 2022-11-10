# Installing GeoPandas on Apple Silicon

When dealing with geo-spatial data, it becomes incredibly useful to be able to plot and visualise your data on a map. While there are alternative routes such as using [Plotly](https://plotly.com/python/maps/) or [Folium](https://python-visualization.github.io/folium/), there is a high likely hood that sooner or later you will run into the need to interact with some form of `.geojson` file. 

That is where [GeoPandas](https://geopandas.org/en/stable/index.html) comes in. GeoPandas is an open source project aiming to make working with geospatial data in python easier. It extends the datatypes used by Pandas to allow spatial operations on geometric types. Geometric operations are performed by [Shapely](https://shapely.readthedocs.io/en/stable/project.html), with further dependance on [Fiona](https://fiona.readthedocs.io/en/latest/manual.html) for file access and [Matplotlib](https://matplotlib.org) for plotting.

The issue however arises when we attempt to install GeoPandas on a M1 mac. While GeoPandas itself is written in pure Python, it depends on other libraries that are written in other languages (C, C++) that need to be compiled specifically for M1 chips. There are three possible sources of required libraries - pip wheels, conda-forge, and Homebrew.

## Using Conda Forge
Conda Forge is a community led GitHub organisation that contains repositories of conda recipes and provides conda packages for a wide range of software.

An M1 compatible version of of miniforge or mambaforge can be found [here](https://github.com/conda-forge/miniforge).

Once you have set this up, installing geopandas is as simple as running the command:
```
$ conda install -c conda-forge geopandas pygeos
```

Note: if you install x86 (Intel) version of conda, it will run under Rosetta2 and install all packages using x86 architecture, meaning that everything will run under emulation. Try to avoid that.

## Using Homebrew and Pip
As we discussed [here](../core/Python.md), Homebrew is a package management system that installs packages to their own directory and then symlinks their files into `/usr/local`. As a result of this, Homebrew is able to install C libraries compiled for the M1 chip set. 

To install GeoPandas, work through the following terminal commands to manually install the dependencies. 

### Shapely
```
$ brew install geos
$ export DYLD_LIBRARY_PATH=/opt/homebrew/opt/geos/lib/
```
Then in your desired environment 
```
$ <env_name> pip install shapely
```

### Fiona:
```
$ brew install gdal
```
In your environment 
```
$ <env_name> pip install fiona
```

### Pyproj
[Proj](https://proj.org) is a generic coordinate transformation software that transforms geospatial coordinates from one coordinate reference system (CRS) to another.
```
$ brew install proj
```
In your environment 
```
$ <env_name> pip install pyproj
```

### GeoPandas and pygeos for speedup improvements
```
$ <env_name> pip install pygeos
$ <env_name> pip install geopandas
```