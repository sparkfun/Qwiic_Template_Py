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

The LICENSE File
----------------
The file named LICENSE contains the license for the repository. The name of the file, LICENSE, is used by other systems to identify which license the repository implements.

For example, in GitHub, when the contents of the file is viewed, the system will display details about the license and clearly indicate to the user what the license covers. 

The SparkFun qwiic python module implementations fall under the MIT license. 

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
This is a RST ([reStructured Text](https://gist.github.com/dupuy/1855764)) file that has a simple, high-level desription of the package. When setup.py is executed, it reads the contents of this file and sets it as the description of the package.

#### setup.cfg
This file contains options that the packaging tools use when creating the specific package. For the most part, the file in this template repo can be used. 
