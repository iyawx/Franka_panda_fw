# Franka Controller - testing

**For more information of the franka software setup,  
please refer to `Setup` > `SETUP.md`**
---
**For more information of the prerequisites of controller setup,  
please refer to `Setup` > `NEWCONTROLLER.md`**
---
## To source directories:
```
source /opt/ros/melodic/setup.sh
source ~/ws_moveit/devel/setup.bash
source ~/catkin_ws/devel/setup.bash
```

## joint impedance controller
```
roslaunch franka_panda_controller_swc joint_impedence_controller.launch robot_ip:=172.16.0.2 load_gripper:=true
```
