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
* [Qwiic_Micro_OLED_P - An example of a pythong package, that includes resource files](https://github.com/sparkfun/Qwiic_Micro_OLED_Py)
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
   |      `--- ... example files for the package
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
