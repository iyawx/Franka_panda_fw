# Building New Controller

---

# 1. Create a package under franka_ros
```
cd ~/catkin_ws/src/franka_ros
catkin_create_pkg file_name
```
# 2. The files we need to build
#### `CMAKELists.txt`

#### `package.xml`

#### `file_name.plugin.xml`

#### `config/file_name.yaml`

#### `include/file_name.h`

#### `launch/file_name.launch`

#### `src/file_name.launch`

We can copy `cfg`, `msg` and `scripts` files from franka_example_controllers
