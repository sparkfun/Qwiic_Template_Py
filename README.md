Qwiic_Template_Py
==================
<p align="center">
   <img src="https://cdn.sparkfun.com/assets/custom_pages/2/7/2/qwiic-logo-registered.jpg"  width=200>  
   <img src="https://www.python.org/static/community_logos/python-logo-master-v3-TM.png"  width=240>   
</p>


This is a template repository and associated documentation that outlines how to publish and maintain a python package 
for the SparkFun Qwiic ecosystem.

This repository defines the general structure of a repository and details the role of each file/location in the repository. 
Additionally, the use of ReadTheDocs and PyPi are outlined. 

The general structure implemented follows the guidelines set forth in the python packaging structure and tools. While this document provides a high-level overview of SparkFun's implementation of a python project, details of this process and structure can be found in the [Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/) document provided by the Python Foundation. 

For details and example use of the described structure, please review the existing python projects for the Qwiic system. 
Examples Include:
* [Qwiic_CCS811_Py - An example of a repository for a single file, python module](https://github.com/sparkfun/Qwiic_CCS811_Py)
* [Qwiic_Micro_OLED_Py - An example of a python package, that includes resource files](https://github.com/sparkfun/Qwiic_Micro_OLED_Py)
* [Qwiic_Py - the overall package for the Qwiic python system](https://github.com/sparkfun/Qwiic_Py)

Repository Structure
---------------------

The general structure of a Qwiic Python repository is as follows

```
Qwiic_Template_Py/
   +--- docs/
   |       `--- ... files to support automatic documentation creation via doxygen
   |
   +--- examples/
   |       `--- ... example files for the package
   |
   +--- DESCRIPTION.rst   - Contains the RST formatted description for the package. Used when building the installer package
   +--- LICENSE           - The license for the package. Currently using MIT
   +--- README.md         - The GitHub markdown formatted readme for the package
   +--- pyproject.toml    - Configuration details used when building the installer package.
   |
   +--- qwiic_<mod>.py.   - If a module (single file implementation), the implementation source code file.
   |
   or
   |
   `--- qwiic_package/    - If a package (directory that contains the implementation) the name of the package
        `---  ... implementation files.
   

```
Implementation
-----------------
To keep the implementation simple and minimize resource needs, there are few design requirements when implementing a new Qwiic I2C driver. Each driver implements a class that encapsulates all interations with the underlying I2C device. This class implements a simple interface that enables the higher-level functionality provided by the overarching [Qwiic package](https://github.com/sparkfun/Qwiic_Py).

The specific implementation requirements are as follows

### Device Class
Each development driver package implements a class that encapsulates all interactions with this device. 

#### Class Name
The class name should be a **CamelCase** version of the package name. This naming schema is used by future automation functionality and follows common python methodologies

```
     qwiic_bme280.          -> QwiicBme280
     qwiic_micro_oled       -> QwiicMicroOled
     qwiic_scmd             -> QwiicScmd
     qwiic_my_super_board.  -> QwiicMySuperBoard
```

An example of a class declaration (note the use and location of the class docstring):
```python
class QwiicScmd(object):
	"""
	QwiicScmd

		:param address: The I2C address to use for the device. 
						If not provided, the default address is used.
		:param i2c_driver: An existing i2c driver object. If not provided 
						a driver object is created. 
		:return: The Serial Control Motor Driver device object.
		:rtype: Object
	"""
```

#### Class Variables
To support the dynamic discovery and enumeration of Qwiic boards by the [Qwiic package](https://github.com/sparkfun/Qwiic_Py), each object implements a set of class variables. This allows the Qwiic package to inspect these values at runtime without having to instantiate an actual object. 

These variables are:

| Class Variable Name| Description|
|----|----|
|**device_name**      |      - Set to the human-readable name of the device|
|**available_addresses**|   - Set to an array of the I2C addresses this device supports. The first address is the default|

These values are set outside of any class method, by convention they are placed right after the class declaration statement. 

Example:
```python
class QwiicScmd(object):
	"""
	QwiicScmd

		:param address: The I2C address to use for the device. 
						If not provided, the default address is used.
		:param i2c_driver: An existing i2c driver object. If not provided 
						a driver object is created. 
		:return: The Serial Control Motor Driver device object.
		:rtype: Object
	"""
	device_name = "Qwiic Serial Control Motor Driver"
	
        # note, the first address is the default I2C address.
	available_addresses = [0x58, 0x59, 0x5A, 0x5C]
```
#### The Constructor 
The Qwiic package expects the constructor of the class to implement the following signature:
```
def __init__(self, address=None, i2c_driver=None):
```
The method supports the following parameters:

|Parameter | Description |
|----|----|
|address| The I2C address to use for the device. If not provided, the default address is used|
|i2c_driver| An existing Qwiic I2C device object. If not provided, the class should create an instance of driver|

The initial body of the constructor handles these parameters - setting the I2C address and constructing a I2C driver if needed. The following object constructor provides a *boilerplate* implementation for this functionality. 

```python
def __init__(self, address=None, i2c_driver=None):


		# Did the user specify an I2C address?
		self.address = address if address != None else self.available_addresses[0]

		# load the I2C driver if one isn't provided

		if i2c_driver == None:
			self._i2c = qwiic_i2c.getI2CDriver()
			if self._i2c == None:
				print("Unable to load I2C driver for this platform.")
				return
		else:
			self._i2c = i2c_driver
```
Note - the docstring for the constructor is actually the docstring for the class.

### Interface Conventions
While not strictly required, the following conventions and patterns are used for qwiic driver implementations

#### Device Constants as Class Attributes
A standard methodology for I2C device implementations is to define constants (#defines in C/C++) for I2C interaction values for a device. For Qwiic python modules these values are defined as capitalized attributes and either placed as file attributes or class attributes on the driver class. 

The convention is to implement any attributes required for user interaction as class attributes. Any internal values are created as file/modules attributes. 

#### is_connected() Method
Each class implements an ```is_connected()``` method that returns True the specific Qwiic device is connect to the system. 

This is a standard method, that often uses the following implementation pattern.
```python
def is_connected(self):
		""" 
			Determine if a SCMD device is connected to the system.

			:return: True if the device is connected, otherwise False.
			:rtype: bool

		"""
		return qwiic_i2c.isDeviceConnected(self.address)
	
# expose as a property
connected = property(is_connected)
```

Additionally, the method is exposed as a read-only attribute on the object. 

#### A begin() Method
Following the pattern set by the Qwiic Arduino libraries, a begin() method is used to perform the actual initialization of the underlying I2C device. 

While each device implements device specific initialization logic, the signature of this method is as follows:
```python
def begin(self):
		""" 
			Initialize the operation of the SCMD module

			:return: Returns true of the initialization was successful, otherwise False.
			:rtype: bool

		"""
```


Implementation Structure
-------------------------
There are two patterns of implementation for a package - a python module or a python package. 

To the end-user a package or module looks the same, but the implementation within the repository is different. 

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

The package directory is name is the name of the package. A file named ```__init__.py``` in the root directory of the package defines its entry/operation and lets python know that the directory implements a package.

For example, if the user imports a package named qwiic_package
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

Note: The souce of the implementation must be contain the [license attribution statement](#source-code-license-attribution).

The LICENSE File
----------------
The file named [LICENSE](https://github.com/sparkfun/Qwiic_Template_Py/blob/master/LICENSE) contains the license for the repository. The name of the file, LICENSE, is used by other systems to identify which license the repository implements.

For example, in GitHub, when the contents of the file is viewed, the system will display details about the license and clearly indicate to the user what the license covers. 

The SparkFun Qwiic python module implementations fall under the MIT license. 

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
The Qwiic Python components are packaged using standard python package/install tools, and hosted within the Python Package Index (pypy.org).

Within the repository, the files that makeup the package are the following:
```
Qwiic_Example_Py
   + DESCRIPTION.rst          - A high level description of the package
   |
   + pyproject.toml           - Specific options/settings for the package tools 
package

```

#### DESCRIPTION.rst
The file [DESCRIPTION.rst](https://github.com/sparkfun/Qwiic_Template_Py/blob/master/DESCRIPTION.rst) is a RST ([reStructured Text](https://gist.github.com/dupuy/1855764)) file that has a simple, high-level description of the package. When setup.py is executed, it reads the contents of this file and sets it as the description of the package. _(\***Note:** The title should read **Qwiic** <Package Name> and the number of `=` must be greater than the character length of the title.)_

```
Qwiic <Example Package Title>
=============================================
```

#### pyproject.toml
The file [pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)  is a configuration file used by packaging tools, as well as other tools such as linters, type checkers, etc.

This template repository contains an example ```pyproject.toml``` file for review.

Building and Uploading the Package
----------------------------------
SparkFun Qwiic Python Libraries are packaged in three ways: 

1) `PyPi` packaging on pypi.org. This packaging supports native python usage i.e. on RaspberryPi or NVIDIA Jetson platforms. 
2) `Mip` packaging. This packaging supports MicroPython.
3) `Circup` packaging. This packaging supports CircuitPython. 

### PyPi Packaging Instructions

When ready to build and upload a package to pypi.org, the following setups are performed.

#### Get an Account on PyPi.org
You'll need an account on PyPi.org - it's a simple sign up procedure.

To publish a new package, you can use this account. If you are updating or modifying an existing package, you'll need to be added as a *Maintainer* of the package by the package owner.

#### Build the Package distributions

To build and upload the packages, make sure the required python packages are installed - **setuptools**, **twine**, and **wheel**. 
```sh
sudo pip install setuptools twine wheel
```

Build the distribution packages using the following commands (executing in the package root directory). First create a standard distribution:
```sh
py -m build
```

These commands will create distribution package files and place them in the ```./dist``` subdirectory. 

### Upload the Package to PyPi.org

The ```twine``` command is used to upload the install packages to pypi.org. To upload the packages, use the following command:

```sh
py -m twine upload dist/*
```
This command will prompt for the *username* and *password* for the pypi account to use for the upload.

Once the upload is completed, the packages are now available for use via the pip installer. 

NOTE: Your PyPi.org username and password can be specified in the file ```~/.pypirc``` instead of entering with each call to twine. The format of this file:

```ini
[pypi]
username = <the username>
password = <the password>
```

You can alternatively use an API key instead of a username and password.

### Mip Packaging Instructions
Mip packaging utilizes the package.json structure as seen in `package.json` in this repository. Upate the `"urls"` entry with your `.py` implementation file and your repository name. Update the `"deps"` entry with any dependencies that your library relies on. The SparkFun convention is to also include a branch called "examples" in each Qwiic Python repository that is exactly the same as the main/master branch, but with a different package.json. The package.json in the example branch should contain the paths to all examples in the `"urls"` entry. This will allow users to install the main package on a MicroPython board with: 

`mpremote mip install github:sparkfun/qwiic_your_package_py`

And will allow users to install the examples package with:

`mpremote mip install --target "" github:sparkfun/qwiic_your_package_py@examples`

Just by nature of having the package.json checked into the repo (and having your repo public), it is installable with mip, no other packaging or publishing is required. 

### Circup Packaging Instructions

For packaging with circup, first create a tagged release of your repository. Then, add your repository as a submodule to the [Qwiic_Py repository](https://github.com/sparkfun/Qwiic_Py) as described below in the [Adding the Module to Qwiic Py](#adding-the-module-to-qwiic_py) section below. Next, run the [update-submodules.sh](https://github.com/sparkfun/Qwiic_Py/blob/main/update-submodules.sh) script in the Qwiic_Py repository. This will pull the latest releases from all of the libraries in Qwiic Py. Then, add and commit any updates after `update-submodules.sh` is run. Finally, create a new release in Qwiic_Py. Name the release "Month Year Release" and name the tag for the release "YYYYMMDD". For example a release on February 27, 2026 would be named `February 2026 Release` and have the tag `20260227`. When your release is published, the [build.yml](https://github.com/sparkfun/Qwiic_Py/blob/main/.github/workflows/build.yml) and [release.yml](https://github.com/sparkfun/Qwiic_Py/blob/main/.github/workflows/release.yml) workflows will run and your package will be published for Circup! To intall your package with circup, run the following on a CircuitPython board: 

```
circup bundle-add sparkfun/Qwiic_Py
circup install --py qwiic_ens160
```

Documentation Generation - Doxygen
------------------------------------------

See the `docs/doxygen` directory for the doxygen files used in the auto-generation of doxygen docs. See the `.github/workflows` directory for the workflows that will utilize these doxygen files to generate github pages documentation. The key files that you must edit when creating a new library are `docs/doxygen/doxygen-custom/header.html` and `docs/doxygen/doxygen-config`. Within these files, simply search (ctrl+F) for the `TODO` (and `todo`) and replace each `TODO` with your correct module name instead of `TODO` i.e. `ENS160`. 

To accurately generate the doxygen documentation, your implementation file requires doxygen-style comments. See the `qwiic_template.py` file for an example. 

NOTE: In your GitHub Repo you must also:
1) Navigate to `Settings > Actions > General > Actions Permissions` and check the radio button for `Allow all actions and reusable workflows`
2) Navigate to `Settings > Pages > Build and deployment` and select the `source` as `GitHub Actions`.
3) Navigate to `Settings > Environments > github-pages > Deployment branches and tags` and make sure the branch you wish to deploy doxygen from is included in the list of allowed branches.

Adding the Module to Qwiic_Py
------------------------------------------------------------------

The overall Qwiic package, which is hosted in the Qwiic_Py repository, defines dependencies to all the SparkFun Qwiic python packages. This is accomplished by adding modules to the repo as git submodules. 

New drivers are added as git submodules in the Qwiic_Py/qwiic/drivers directory. 

Naming of the driver directory is important – it should map to the package name in PyPi, minus the initial ‘sparkfun-‘ name. 

So for the BME280 package, which is defined in PyPi as ‘sparkfun-qwiic-bme280’, would be placed in a directory named ‘qwiic_bme280’ in the drivers folder. 

To add a driver/package to the Qwiic repository, do the following steps:
* Clone the Qwiic repository 
```git clone git@github.com:sparkfun/Qwiic_Py.git```
* Move to the drivers directory
```cd Qwiic_Py/qwiic/drivers```
* Add the submodule, using the following command
``` git submodule add <repo to add> <name of driver folder>```

Example for the Titan GPS driver
``` git submodule add git@github.com:sparkfun/Qwiic_Titan_Gps_Py.git qwiic_titan_gps ```

_Note, if you get a failure due to permissions, you may need to use the complete URL for the \<repo to add\>. (\***Note:** Don't forget to include the submodule name (`qwiic_<package_name>`) after the link for the repo.)_

Example for the Titan GPS driver (with full URL)
``` git submodule add https://github.com/sparkfun/Qwiic_Titan_Gps_Py qwiic_titan_gps ```

* Add this new folder to the repo, commit it and push to GitHub 

Once completed, the Qwiic_Py package must be updated and uploaded to PyPi. 

* Bump up the version in the pyproject.toml file. This step defines package dependencies for everything contained in the drivers subfolder, including the newly added submodule. 
* Follow the above package build and upload steps

Once completed, an update/install of the sparkfun-qwiic package will include the new submodule

README_Template.py
------------------------------------------------------------------
Delete the README.md file that you are reading right now, and replace it with the README_Template.md file in this repository (you should rename README_Template.py to README.md). Replace the `TODO`'s in that template README with the proper device name, capabilities or code. Also, update examples/README.md with necessary documentation for each example with links to the doxygen for functions showcased. 