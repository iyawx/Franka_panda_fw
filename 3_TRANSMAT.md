### The transformation matrix for the default configuration is:
[0.9929938777251041, -0.05883267120326896, 0.10238467903332704, 0.0,  
 -0.060831873300855155, -0.998001797123256, 0.016511905724919125, 0.0,  
 0.10121060277042856, -0.022624908716925553, -0.9946077253834285, 0.0,  
 0.2705895741045692, -0.0029785730994902346, 0.5246570936196611, 1.0]  

### To collect the transfomation matrices:
```
roslaunch franka_example_controllers force_example_controller.launch robot_ip:=172.16.0.2 load_gripper:=true
```
### Open another terminal and run:
```
rostopic echo /franka_state_controller/franka_states/O_T_EE
rostopic echo /franka_state_controller/franka_states/elbow
```
