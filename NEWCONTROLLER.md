# Building New Controller
To understand all the essential files of the controllers, we can start with writing a new controller by copying one of the example controllers into a new file.  
Make sure you installed `franka_ros` and `libfranka` in you workspace before you build your controllers. For setput instruction, please refer to `SETUP.md`.
---

# 1. Create a package under franka_ros
```
cd ~/catkin_ws/src/franka_ros
catkin_create_pkg file_name
```
# 2. The files we need to build
#### `CMAKELists.txt`
The file CMakeLists.txt is the input to the CMake build system for building software packages. Any CMake-compliant package contains one or more CMakeLists.txt file that describe how to build the code and where to install it to. The CMakeLists.txt file used for a catkin project is a standard vanilla CMakeLists.txt file with a few additional constraints. 
URL: http://wiki.ros.org/catkin/CMakeLists.txt

#### `package.xml`
The package manifest is an XML file called package.xml that must be included with any catkin-compliant package's root folder. This file defines properties about the package such as the package name, version numbers, authors, maintainers, and dependencies on other catkin packages.
URL: http://wiki.ros.org/catkin/package.xml

#### `file_name_plugin.xml`
plugin.xml can open a library containing exported classes at any point without the application having any prior awareness of the library or the header file containing the class definition.
URL: http://wiki.ros.org/pluginlib/Tutorials/Writing%20and%20Using%20a%20Simple%20Plugin  

**In the first line: `<library path="lib/libfile_name">`, the file_name must be exactly the same as your filename. Do not name it ramdomly**

#### `config/file_name.yaml`

#### `include/file_name.h`

#### `launch/file_name.launch`

#### `src/file_name.cpp`

We can copy `cfg`, `msg` and `scripts` files from franka_example_controllers
