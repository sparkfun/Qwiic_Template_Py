Qwiic_Template_Py
==================
<p align="center">
   <img src="https://cdn.sparkfun.com/assets/custom_pages/2/7/2/qwiic-logo-registered.jpg"  width=200>  
   <img src="https://www.python.org/static/community_logos/python-logo-master-v3-TM.png"  width=240>   
</p>


This is a template repository and associated documentation that outlines how to publish and maintain a python package 
for the SparkFun qwiic ecosystem.

This repository defines the general structure of a repository and details the role of each file/location in the repository. 
Additionally, the use of ReadTheDocs and PyPi are outlined. 

The general structure implemented follows the guidelines setforth in the python packaging structure and tools. While this document provides a high-level overview of SparkFun's implementation of a python project, details of this process and structure can be found in the [Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/) document provided by the Python Foundation. 

For details and example use of the described structure, please review the existing pythong projects for the qwiic system. 
Examples Include:
* [Qwiic_CCS811_Py - An example of a repository for a single file, python module](https://github.com/sparkfun/Qwiic_CCS811_Py)
* [Qwiic_Micro_OLED_Py - An example of a pythong package, that includes resource files](https://github.com/sparkfun/Qwiic_Micro_OLED_Py)
* [Qwiic_Py - the overall package for the qwiic python system](https://github.com/sparkfun/Qwiic_Py)

Repository Structure
---------------------

The general structure of a qwiic Python repository is as follows

```
Qwiic_Template_Py/
   +--- docs/
   |       `--- ... files to support automatic documentation creation via readthedocs.org
   |
   +--- examples/
   |       `--- ... example files for the package
   |
   +--- .readthedocs.yml  - configuration file for readthedocs
   +--- DESCRIPTION.rst   - Contains the RST formatted description for the package. Used when building the installer package
   +--- LICENSE           - The license for the package. Currently using MIT
   +--- README.md         - The GitHub markdown formatted readme for the package
   +--- setup.cfg         - Configuration details used when building the installer package.
   +--- setup.py          - The python script used to define and build the python installer package.
   |
   +--- qwiic_<mod>.py.   - If a module (single file implemntation), the implementation source code file.
   |
   or
   |
   `--- qwiic_package/    - If a package (directory that contains the implementation) the name of the package
        `---  ... implementation files.
   

```

Implementation
--------------
There are two patterns of implementation for a package - a python module or a python package. 

To the end user a package or module looks the same, but the implementation within the repository is different. 

### Module
A python module is nothing more than a single file that makes up the overall implementaiton for the package. 
This file has the same name as the package being imported by the user. 

For example, if a user imports a module named qwiic_module
```python
import qwiic_module
```
The file name would be ```qwiic_module.py``` and reside in the root of the repository.
```
Qwiic_Module_Py/
   +--- qwiic_module.py
```

### Package
A python package is a folder that contains the implementation of the package. The folder can contain python source files, as well as any other resource needed for the property operation of the package.

The pacakge directory is name is the name of the package. A file named ```__init__.py``` in the root directory of the package defines it's entry/operation and let's python know that the directory implements a package.

For example, if the user imports a pacakge named qwiic_package
```python
import qwiic_package
```

The structure of this implementation would be under a directory called ```qwiic_package``` in the repository

```
Qwiic_Package_Py
   +--- qwiic_package/
           +--- __init__.py       - the entry point for the package implementation
           |
           +---  ... Any other file, directory or resource that makes up the package
```

Note: The souce of the implementation must be contain the [license attribution statement](#source-code-license-attribution)

The LICENSE File
----------------
The file named [LICENSE](https://github.com/sparkfun/Qwiic_Template_Py/blob/master/LICENSE) contains the license for the repository. The name of the file, LICENSE, is used by other systems to identify which license the repository implements.

For example, in GitHub, when the contents of the file is viewed, the system will display details about the license and clearly indicate to the user what the license covers. 

The SparkFun qwiic python module implementations fall under the MIT license. 

### Source Code License Attribution
Each source file distributed with one of our python packags/modules must include the proper license attribution in the entry comment section of the code. 

The Qwiic Python packages are licensed using the MIT license and as such should include the following statement in the top/entry section of the code:
```python
#==================================================================================
# Copyright (c) 2019 SparkFun Electronics
#
# Permission is hereby granted, free of charge, to any person obtaining a copy 
# of this software and associated documentation files (the "Software"), to deal 
# in the Software without restriction, including without limitation the rights 
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell 
# copies of the Software, and to permit persons to whom the Software is 
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all 
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, 
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE 
# SOFTWARE.
#==================================================================================
```

Installer/Package Creation
--------------------------
The qwiic Python components are pacakged using standard python package/install tools, and hosted within the Python Package Index (pypy.org).

Within the repository, the files that makeup the package are the following:
```
Qwiic_Example_Py
   + DESCRIPTION.rst          - A highlevel description of the package
   |
   + setup.cfg                - Specific options/settings for the package tools 
   |
   + setup.py                 - A python script that defines and builds the installer package

```

#### DESCRIPTION.rst
The file [DESCRIPTION.rst](https://github.com/sparkfun/Qwiic_Template_Py/blob/master/DESCRIPTION.rst) is a RST ([reStructured Text](https://gist.github.com/dupuy/1855764)) file that has a simple, high-level desription of the package. When setup.py is executed, it reads the contents of this file and sets it as the description of the package.

#### setup.cfg
The file [setup.cfg](https://github.com/sparkfun/Qwiic_Template_Py/blob/master/setup.cfg) contains options that the packaging tools use when creating the specific package. For the most part, the file in this template repo can be used. 

### setup.py
The file ```setup.py``` is a python script that is used to describe the package and build an install package. The file is used by the python package ```setuptools```, which is a collection of untilities that make it simple to build and distribute Python distributions. 

This template repository contains an example ```setup.py``` file for review and an overview of the file contents are below. For details on the strucutre of the file, please review the [setup.py section](https://packaging.python.org/tutorials/packaging-projects/#creating-setup-py) in the Packaging Python Projects document. 

___Description Section___

One of the first sections in ```setup.py``` is reading in the contents of *DESCRIPTION.rst*. 
```python
import io

here = path.abspath(path.dirname(__file__))

# get the log description
with io.open(path.join(here, "DESCRIPTION.rst"), encoding="utf-8") as f:
    long_description = f.read()
```
This reads the contents of the description fine and places the resultant string into the variable ```long_description```. This variable is passed into the call to ```setup()``` using the *```long_description```* keyword parameter.

Note: The ```io.open``` method is used to support *uft-8* file encoding in Python versions 2.7 and 3.*. 

__setuptools.setup( name=)___

This keyword is set to the name to publish the package under in PyPi.org. This is the name passed to the ```pip``` command for installing the package.

The following command shows this value for the qwiic bme280:
```python
setuptools.setup(
     # ...
    name='sparkfun_qwiic_bme280',
   # ...
   )
```

**NOTE:** For PyPi/Pip, underscores ```_``` and dashes ```-``` are interchangeable. 

___setuptools.setup( install_requires=)___

The *```install_requires```* keyword arguement to ```setuptools.setup()``` is used to specify what other python packages this package depends on. 

An example of this is the ```sparkfun-qwiic-i2c``` package, which all qwiic board python packages use. And example of this from the qwiic Proximity package ```setup()``` is as follows:
```python
setuptools.setup(
     # ...
     
    install_requires=['sparkfun_qwiic_i2c'],

   # ...
   )
```
For the overall qwiic package, which depends on all driver packages, this parameter has the following form:
```python
setuptools.setup(
     # ...
    
    setup_requires = ['sparkfun_qwiic_ccs811', 'sparkfun_qwiic_bme280', \
            'sparkfun_qwiic_micro_oled', 'sparkfun_qwiic_proximity', \
            'sparkfun_qwiic_scmd', 'sparkfun_qwiic_i2c']

   # ...
   )
```

___setuptools.setup(classifiers=[])___

The classifiers argument to ```setup()``` are attrbitues that describe the package and are used details specifics to the PyPi respository and users of the project. While a [detailed list of of valid classifier values](https://pypi.org/pypi?%3Aaction=list_classifiers) is available at pypy.org, the key values are the project maturity (is it Alpha, Beta, Production?) and what python versions are supported. 

The example script has the following classifiers:
```python
setuptools.setup(
     # ...
    # See https://pypi.python.org/pypi?%3Aaction=list_classifiers
    classifiers=[
        # How mature is this project? Common values are
        #   3 - Alpha
        #   4 - Beta
        #   5 - Production/Stable
        'Development Status :: 5 - Production/Stable',

        # Indicate who your project is intended for
        'Intended Audience :: Developers',
        'Topic :: Software Development :: Build Tools',

        # Pick your license as you wish (should match "license" above)
        'License :: OSI Approved :: MIT License',

        # Specify the Python versions you support here. In particular, ensure
        # that you indicate whether you support Python 2, Python 3 or both. 
        'Programming Language :: Python :: 2.7',
        'Programming Language :: Python :: 3.5',
        'Programming Language :: Python :: 3.6',
        'Programming Language :: Python :: 3.7',
   
    ],
    # ...
   )
```

You can see these detailed out on the SparkFun Qwiic package (sparkfun-qwiic) on the [PyPi.org repository.](https://pypi.org/project/sparkfun-qwiic/)

___setuptools.setup(packages=[])___

If your repository defines one or more packages (directories), the names of these packages are provided to the ```packages``` keyboard argument to setuptools. Note: this is the directory/package name the user references in python code, not the package name used by PyPi - which can also contain additional keywords. 

For the qwiic package, this is just the qwiic directory:
```python
setuptools.setup(
     # ...
     
    packages=['qwiic'],

   # ...
   )
```
or for the qwiic_micro_oled package, which includes a font subpackage:
```python
setuptools.setup(
     # ...
     
    packages=["qwiic_micro_oled", "qwiic_micro_oled/fonts"],

   # ...
   )
```
___setuptools.setup(package_data={})___

The packaging system will include python files (```.py```) files by default. If the package includes non-python files, these are specified via the ```pacakge_data``` keyword argument, which takes a dictionary. 

The provided dictionary key values are a specific location, and the value is the data files to include in the package. The data filenames can be specific names, or incldue wildcards. 

An example of this is used in the qwiic_micro_oled package, which includes font data files, named using a ```.bin``` file extension. These data files are located in the ```./fonts``` subdirectory of the pacakge repository.
```python
setuptools.setup(
     # ...
     package_data={
         "qwiic_micro_oled/fonts" : ['*.bin']
    },

   # ...
   )
```
___setuptools.setup(py_modules=[])___

If the install package implements a modudule (source file) and not a python package (directory), the modules are specific to the ```setup()``` method call using the ```py_modules=[]``` keyword argument. The value of this keyword is an array that contains the names of the modules to include in the package. Note, the file suffix is not included in the provided names.

For the qwiic BME280 package, which is implemented in a single file, the module is specified as follows:
```python
setuptools.setup(
     # ...
    py_modules=["qwiic_bme280"],
   # ...
   )
```

Building and Uploading the Package
----------------------------------

When ready to build and upload a package to pypi.org, the following setups are performed.

#### Get an Account on PyPi.org
You'll need an account on PyPi.org - it's a simple sign up procedure.

To publish a new package, you can use this account. If you are updating or modifiying an existing package, you'll need to be added as a *Maintainer* of the package by the package owner.

#### Build the Package distributions

To build and uplaod the pacakges, make sure the required python packages are installed - setuptools and twine. 
```sh
sudo pip install setuptools twine
```

Build the distribution packages using the following commands (executing in the package root directory). First create a standard distribution:
```sh
python setup.py sdist 
```
Then a distribution in the ```wheel``` format.
```sh
python setup.py bdist_wheel --universal
```
These commands will create distribution package files and place them in the ```./dist``` subdirectory. 

### Upload the Package to PyPi.org

The ```twine``` command is used to upload the install packages to pypi.org. To upload the pacakges, use the following command:

```sh
twine upload dist/*
```
This command will prompt for the *username* and *password* for the pypi account to use for the upload.

Once the upload is completed, the packages are now available for use via the pip installer. 

NOTE: Your PyPi.org username and password can be specified in the file ```~/.pypirc``` instead of entering with each call to twine. The format of this file:

```ini
[pypi]
username = <the username>
password = <the password>
```
Documentation Generation - ReadTheDocs.org
------------------------------------------

For each pacakge, the documentation is automatically generated and published using readthedocs.org. The documentation itself is generated using [Sphinx](http://www.sphinx-doc.org/), which creates the desried documentation HTML output based on source code comments and documentation structure files. 

The repository files used to define and create the documentation are the following:
```
Qwiic_Template_Py/
   +--- docs/
   |       +--- conf.py      - defines the input and output behavior of Sphinx
   |       +--- index.rst    - Top level documentation structure definition 
   |       +---     *.rst    - Defines additional pages for the documentation - index.rst references these files.
   |       +--- requirements.txt   - Lists the python modules the package depends on. 
   |       `--- _static/
   |                `--- favicon.ico    - the favicon to use for the generated HTML text
   |
   |--- .readthedocs.yml   - Defines how the ReadTheDocs environment is setup when building the documentation
```


To create a packages documentation, the general development pattern is as follows:
* Documentation is added to the python source code using [docstrings](https://www.python.org/dev/peps/pep-0257/)
* Documentation configuration is define in ```docs/conf.py```
* Package depenancies are added to ```docs/requirements.txt```
* The file ```index.rst``` is updated for the desired documentation structure. Addtional ```.rst``` files are added as needed.
* Documentation is generated locally using Sphinx for testing
* Validated documentation files are checked into the GitHub repository.
* The GitHub repository is imported and linked to a project in ReadTheDocs.org
* ReadThe docs generates and publishes the documentation.
    * The documentation is updated automatically when the project is updated on GitHub
    
___Python Docstrings___

Python has a built-in language feature used to embedd documentation strings in code, a feature called _docstrings_.

From the docstring spec (PEP-0257):
> A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. Such a docstring becomes the \__doc__ special attribute of that object.

These strings are enclosed by triple quotes. While the format of the text is left to the user, the Sphinx system will honor reStructured text formatting. The qwiic system python documentation formats the docstrings as reStructure Text.

An example of a docstring is the class definition for the QwiicBME280 object. 
```python
class QwiicBme280(object):
	"""
	QwiicBme280

		:param address: The I2C address to use for the device. 
						If not provided, the default address is used.
		:param i2c_driver: An existing i2c driver object. If not provided 
						a driver object is created. 
		:return: The BME280 device object.
		:rtype: Object
	"""
```
Note that the docstring is the first statement after the class definition. 

Doc strings for methods following the method declaration statement:
```python
def setFilter(self, filterSetting):
		"""
		Set the filter bits in the BME280s config register

		:param filterSetting: The filter bits for the BME280. Acceptable values
						0 = filter off
	  					1 = coefficients = 2
	  					2 = coefficients = 4
	  					3 = coefficients = 8
	  					4 = coefficients = 16

		:return: No return value 

		"""
```

And you can even add docstrings to a module (file) - placing the docstring at the top (first non-comment) of the file.
```python
#==================================================================================

"""
qwiic_bme280
============
Python module for the qwiic bme280 sensor, which is part of the [SparkFun Qwiic Environmental Combo Breakout](https://www.sparkfun.com/products/14348)

This python package is a port of the existing [SparkFun BME280 Arduino Library](https://github.com/sparkfun/SparkFun_BME280_Arduino_Library)

This package can be used in conjunction with the overall [SparkFun qwiic Python Package](https://github.com/sparkfun/Qwiic_Py)

New to qwiic? Take a look at the entire [SparkFun qwiic ecosystem](https://www.sparkfun.com/qwiic).

"""
#-----------------------------------------------------------------------------

import math
import qwiic_i2c
```

The Sphinx documentation package uses the values of the ```__doc___``` attribute of objects to locate and generate documentation. As such, the Sphinx system (either run locally or by ReadTheDocs.org) will instatiate the package as part of the documentation generation process. As such, the depenancies and needs of the package must be defined in the documentation configuration files.

___docs/conf.py___

The ````docs/conf.py``` file sets the configuration Sphinx will use when building the documentation set. Key areas of this file are:

*Project Information*

These are variables that describe the project. The key value being the name of the project.
```python
project = 'sparkfun_qwiic_bme280'
copyright = '2019, SparkFun Electronics'
author = 'SparkFun Electronics'
```

*Project Extensions*

This is an array that contains the names of extensions to the Sphinx system to geneate the documentation. These are python packages that should be installed on your system (for local documentation builds). For the qwiic python packages, the following statement is used:
```python
extensions = [ 
	'sphinx.ext.autodoc',
	'sphinx.ext.intersphinx',
	'sphinx.ext.viewcode',
	'm2r'
	]
```
The unique package here is ```m2r```, which converts markdown into reStructured text. This allows the documentation system to import the GitHub README.md files into the generated documentation page.

*master_doc*

This variable is set to the master documentation structure file being used. For the qwiic packages, this value is set to ```"index"```, which tells Sphinx to use ```index.rst``` to define the documentation structure. 
```python
master_doc = 'index'
```

*Static Values*

The last value worth noting is the static resources that should be used in documentation generation. For SparkFun documentation we include the favion.ico file from www.sparkfun.com. This file is located at ```docs/_static/favicon.ico``` and documented as follows in the conf.py file. 
```python
# Add any paths that contain custom static files (such as style sheets) here,
# relative to this directory. They are copied after the builtin static files,
# so a file named "default.css" will overwrite the builtin "default.css".
html_static_path = ['_static']

html_favicon = '_static/favicon.ico'
```

___docs/*.rst Files___

The structure and contents of the documentation is oulined using a set of reStructured text (```.rst```) files, with the starting definition contained in ```docs/index.rst```.

*docs/index.rst*

This file defines the entry page for the documentation. For the qwiic documentation, we include the GitHub README.md file, and define a table of contents, which defines other pages of the documentation. 

For the qwiic BME280 python package, this file contains the following:
```rst
.. mdinclude:: ../README.md

Table of Contents
=================

.. toctree::
   :maxdepth: 4
   :caption: Contents:

   self


.. toctree:: 
   :caption: API Reference
   :maxdepth: 3

   apiref 

.. toctree:: 
   :caption: Examples

   ex1
   ex4
   ex5

.. toctree::
   :caption: Other Links

	SparkFun <https://sparkfun.com>
   SparkFun Environmental Combo Breakout CCS811/BME280 <https://www.sparkfun.com/products/14348>
	GitHub <https://github.com/sparkfun/Qwiic_BME280_Py>
	SparkFun qwiic <https://www.sparkfun.com/qwiic>
```

The key elements of this file:

**.. mdinclude:: ../README.md**

This includes the packages README.md file, with ```mdinclude``` noting that the file is formatted in markdown.

**.. toctree:: **

Defines an section in the table of contents, and the content of this section.

For the main page (index.rst):
```rst
.. toctree::
   :maxdepth: 4
   :caption: Contents:

   self
```
Note the section referances ```self```

For the package API:
```rst
.. toctree:: 
   :caption: API Reference
   :maxdepth: 3

   apiref 
```
This section links to the file ```apiref.rst```

And the examples section, which links to three example rst files: ```ex1.rst, ex4.rst, ex5.rst```
```rst
.. toctree:: 
   :caption: Examples

   ex1
   ex4
   ex5
```

*API Documentation*

To document the projects API, the *autodoc* system of Sphinx is used. This system will crawl the specified files/packages and generate documention using the included *docstrings*. 

For a moduled, like ```qwiic_bme280```, the contents of ```apiref.rst``` is:
```rst
API Reference
==============

.. automodule:: qwiic_bme280
   :members:
```
This specifieds that Sphinx should inspect the qwiic_bme280 module, and document the package members. 

Got python packages, such as the ```qwiic_mico_oled``` package, the documentation is contained in module and underlying class, so the rst file file takes a slightly different format:
```rst
API Reference
==============

.. automodule:: qwiic_micro_oled
   :members:


.. autoclass:: QwiicMicroOled
	:members:
```

The Sphinx autodoc feature is extemely useful, and fully documented by the [Sphinx project](http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html).

___Documenting Examples___

Any examples for a package are also included in the documentation. These are defined in individiual rst files, which do nothing more that include the source of the example. 

The following shows the format of an an example rst file, which does nothing more that inline a file, add the filename as the caption of the inline, and formats the included file with line numbers. 
```rst
Basic Operation
---------------------------
.. literalinclude:: ../examples/qwiic_bme280_ex1.py
    :caption: examples/qwiic_bme280_ex1.py
    :linenos:
```

___The .readthedocs.yml File___

This file defines the attributes and packages ReadTheDocs should use when creating the documentation generation environment. As noted above, Sphinx instantiates the contents of the pacakge so that it can access the python docstrings during the generation process. The file ```.readthedocs.yml``` and it's associated file ```docs/requirements.txt``` define the environment to use. 

The contents of ```.readthedocs``` used by the qwiic modules is:
```yml
python:
   version: 3
   install:
      - requirements: docs/requirements.txt
   setup_py_install: true
```
This file tells ReadTheDocs the following:
* Use Python version 3 to generate the docs
* Install the modules contained in ```docs/requirements.txt```
* Install the package/module defined by this repository using a the contents of ```setup.py``` 

The ```docs/requirements.txt``` file contains the modules this package needs to generate documentation. This is used by Sphinx when generating local documentation, and by ReadTheDocs, as noted above in the contents of the ```.readthedocs.yml``` file.

For most qwiic driver packages, this file contains the following:
```txt
m2r
sparkfun-qwiic-i2c
```
The contents specifically being:

* mr2 - the Markdown to reStructureText conversion module
* sparkfun-qwiic-i2 - the I2C driver pacakge used by all qwiic packages.


