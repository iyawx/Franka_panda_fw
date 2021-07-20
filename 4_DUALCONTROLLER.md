## Device setup
#### 1. Robot 1
> robot_id: panda_1  
> robot_ip: 172.16.0.2  
> PC workstation IP: 172.16.0.1  

#### 2. Robot 2
> robot_id: panda_2  
> robot_ip: 173.16.0.2  
> PC workstation IP: 173.16.0.1  
  
_**Ensure the IPs setup is correct**_

---

## For testing the connection
1. Open `dual_arm_cartesian_impedance_example_controller.launch` file in franka_ros/franka_example_controllers  
a. Adding ` default="{panda_1/robot_ip: 172.16.0.2, panda_2/robot_ip: 173.16.0.2}"` after `<arg name="robot_ips"`  
b. Save the file

2. Open the terminal and input following command:
```
roslaunch franka_example_controllers dual_arm_cartesian_impedance_example_controller.launch
```




