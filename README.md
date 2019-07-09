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

___setuptools.setup(classifiers[])___

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

You can see these detailed out on the SparkFun Qwiic package (sparkfun-qwiic) on the [PyPi.ori repository.](https://pypi.org/project/sparkfun-qwiic/)
