# Franka Controller - testing

**For more information of the franka software setup,  
please refer to `Setup` > `SETUP.md`**
---
**For more information of the prerequisites of controller setup,  
please refer to `Setup` > `NEWCONTROLLER.md`**
---
## To compile the file after modified the code:
```
cd catkin_ws 
source /opt/ros/melodic/setup.sh 
rosdep install --from-paths src --ignore-src --rosdistro melodic -y --skip-keys libfranka 
catkin_make -DCMAKE_BUILD_TYPE=Release -DFranka_DIR:PATH=/opt/ros/melodic/lib/libfranka/build 
source devel/setup.sh 
```

## To source directories:
Open the bashrc file:
```
gedit .bashrc
```
Source the commands: 
```
source /opt/ros/melodic/setup.sh
source ~/catkin_ws/devel/setup.bash
source ~/ws_moveit/devel/setup.bash
source ~/catkin_ws/devel/setup.bash
```

## joint impedance controller
```
roslaunch franka_panda_controller_swc joint_impedance_controller.launch robot_ip:=172.16.0.2 load_gripper:=true
```
## froce controller
```
roslaunch franka_panda_controller_swc force_controller_NR.launch robot_ip:=172.16.0.2 load_gripper:=true
```
