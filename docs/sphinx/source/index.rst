.. GeoHexViz documentation master file, created by
   sphinx-quickstart on Thu Oct 21 14:50:39 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to GeoHexViz's documentation!
=====================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

        Plot Builder Module <builder>
        Utils Package <util>
        Errors Module <errors>
        Templates Module <templates>

Further Information
===================

Welcome to GeoHexViz!

Creating geospatial visualizations is often time-consuming and laborious. For example, an analyst must make a variety of design decisions, including which map projection to use, the colour scheme, the basemap, and how to organize the data in layers. One of many software applications may be used to construct the visualization, such as:

+ **ArcGIS** which provides a wide range of capabilities, but requires a paid license and a solid foundation in geospatial information processing;
+ **QGIS** which is free and open source, but like ArcGIS requires in-depth knowledge of geospatial information processing to be used effectively;
+ **D3** which emphasizes web standards rather than a proprietary framework, but requires extensive knowledge of JavaScript; and
+ **Plotly** which is a free and open source Python graphing library, but like D3 and other packages requires knowledge of a programming language.

Common across these applications is the requirement to have knowledge of geospatial concepts, and acquiring this knowledge has been identified as a significant challenge. In addition, the latter two options require programming. While many analysts have programming experience, not all do and in time-sensitive situations, as often encountered in a military setting, writing code to produce a visualization may not be feasible. With this in mind, GeoHexViz aims to reduce the time, in-depth knowledge, and programming required to produce publication-quality geospatial visualizations that use hexagonal binning. Implemented in Python, it seamlessly integrates several underlying packages &emdash; Pandas, GeoPandas, Uber H3, Shapely, and Plotly &emdash; and extends their functionality to achieve these goals. Although originally designed for use within the military operations research community, it may be used in other research communities.


Example Usage
#############
GeoHexViz allows a user to generate hexagonally binned geospatial visualizations with two different methods.
Method 1 concerns using the GeoHexSimple package's script to run a file containing plot structure.
Method 2 concerns using Python code to interact with the functions within the package.
Method 2 method has two categories:

a) Using functions from the GeoHexSimple package \
b) Using functions from the GeoHexViz package

Please refer to the `examples directory <https://github.com/tony-zeidan/geohexviz/blob/master/examples>`_ for additional examples that go into great depth (for both methods).

Method 1 Example Usage
**********************

The GeoHexViz distribution includes a module that can allow the reading
of JSON files for quick and easy plots.

.. code-block:: json

    {
      "hexbin_layer": {
        "data": "<sample csv file>",
        "hex_resolution": 4
      },
      "output": {
        "filepath": "<sample filepath>",
        "width": 600,
        "height": 400
      },
      "display": true
    }

Running the JSON script will allow you to input a JSON file via command-line.
The GeoHexSimple command-line script was created using argparse and is very robust.
Running the help command provides the following:

.. code-block::

        >geohexsimple --help
        usage: geohexsimple [options]

        Input plot property files to make hexagonally binned plots.

        optional arguments:
          -h, --help            show this help message and exit
          -p PATH, --path PATH  path to json file or directory containing json files (required if no gui is used)
          -g, --gui             enable command-line gui (set to true if no path is provided)
          -nf, --nofeedback     turn off feedback while plotting
          -v, --verbose         whether to raise all errors or not


Running your plot properties file may look something like:

.. code-block::

    >geohexsimple --path <path to file>
    exit

Or something like:

.. code-block::

    >geohexsimple

    =================GeoHexSimple================
     A script for the simple creation of
     hexagonally binned geospatial visualizations.
    =============================================
    Main Menu
    Please input the location of your parameterized
    builder file (JSON, YAML) or a directory containing
    builder files.
    Options: file path, help, exit.
    <path to file>

Method 2
********
As previously mentioned there are two ways to use the GeoHexViz library in Python code.
Method 2a concerns using the functions that the GeoHexSimple script uses to create plots from pre-existing plot parameter files.
Method 2b concerns using the functions from the GeoHexViz package to create plots.

Method 2a Example Usage
_______________________
You can use the functions that the GeoHexSimple script uses to create a plot from a pre-existing plot parameter file.
A simple example of this method is given below.

.. code:: python

    from geohexviz.utils.file import run_json

    run_json("<filepath here>")

Method 2b Example Usage
_______________________
You can use the functions and objects within GeoHexViz to create a plot from scratch.
A simple example of this method is given below.

.. code:: python

    from pandas import DataFrame
    from geohexviz.builder import PlotBuilder

    # Creating an example dataset
    inputdf = DataFrame(dict(
        latitude=[17.57, 17.57, 17.57, 19.98, 19.98, 46.75],
        longitude=[10.11, 10.11, 10.12, 50.55, 50.55, 31.17],
        value=[120, 120, 120, 400, 400, 700]
    ))

    # Instantiating builder
    builder = PlotBuilder()
    builder.set_hexbin(inputdf, hexbin_info=dict(binning_fn='sum', binning_field='value'))

    builder.finalize(raise_errors=False)
    builder.display(clear_figure=True)

    # A mapbox map
    builder.set_mapbox('<ACCESS TOKEN>')
    builder.finalize()
    builder.display(clear_figure=True)


Installation
############

There are a few steps that a user must follow when installing GeoHexViz.
First, the user must install GeoPandas.
This is most easily done through the use of Anaconda, with this tool it can be installed like this:


.. code-block::

    conda install -c conda-forge geopandas


The version that GeoHexViz was developed with is version 0.8.1 (build py_0).
Next, the user must download or clone GeoHexViz's GitHub repository.
Finally, the user can navigate to the directory containing the ``setup.py`` file, and run:


.. code-block::

    python setup.py install

Or

.. code-block::

    pip install .

Note that to use the pdf cropping features, the user can do an editable install:

.. code-block::

    pip install -e .[pdf-crop]

The user may also install using pip and GitHub:

.. code-block::

    pip install git+https://github.com/tony-zeidan/geohexviz.git


Setting up a conda environment first helps.
To make this process smoother the ``environment.yml`` file is included, which includes all dependencies.
With this file, the first step (installation of GeoPandas) is done automatically.
Using this file an environment can be set up like this:

.. code-block::

    conda env create -f environment.yml

This will create an Anaconda environment called ``geohexviz`` on your machine,
simply activate the environment and run the ``setup.py`` file as shown above.

Further Documentation
#####################

The official documentation for GeoHexViz can be found at `this page <https://github.com/tony-zeidan/geohexviz/blob/master/docs>`_.
The reference document published alongside this package can also be seen in the `docs directory <https://github.com/tony-zeidan/geohexviz/blob/master/docs>`_.

Limitations
###########

This package uses GeoJSON format to plot data sets. With GeoJSON comes
difficulties when geometries cross the 180th meridian . The issue
appears to cause a color that bleeds through the entire plot and leaves
a hexagon empty. In the final plot, this issue may or may not appear as
it only occurs at certain angles of rotation. In this package a simple
solution to the problem is implemented, in the future it would be best
to provide a more robust solution. The solution that is used works
generally, however, when hexagons containing either the north or south
pole are present, the solution to the 180th meridian issue persists.
This pole issue can be seen below.

There also exists some issues with the generation of discrete color
scales under rare circumstances. These circumstances include generating
discrete color scales with not enough hues to fill the scale, and
generating diverging discrete colorscales with the center hue in a weird
position. These issues have been noted and will be fixed in the near
future.

There exists issues with the positioning and height of the color bar
with respect to the plot area of the figure. Although the user is
capable of altering the dimensions and positioning of the color bar,
this should be done automatically as it is a common feature of
publication quality choropleth maps.

Contributing
############

For major changes, please open an issue first to discuss what you would like to change.
For more details please see `this page <https://github.com/tony-zeidan/geohexviz/blob/master/CONTRIBUTING.md>`_.

Acknowledgements
################

Thank you to Nicholi Shiell for his input in testing, and providing
advice for the development of this package.

Contact
#######

For any questions, feedback, bug reports, feature requests, etc please
first present your thoughts via GitHub issues. For further assistance
please contact tony.azp25@gmail.com.

Copyright and License
#####################

For license see `this page <https://github.com/tony-zeidan/geohexviz/blob/master/LICENSE>`_.

Copyright (c) Her Majesty the Queen in Right of Canada, as represented
by the Minister of National Defence, 2021.
