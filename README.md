# **This branch is for NeuroRehack 2021 project 2**  

## Connecting ROS and Unity
URL:http://wiki.ros.org/rosbridge_suite  

---
### Download ROS bridge:
```
sudo apt-get install ros-melodic-rosbridge-suite
```
### Connecting the ROS with Unity via ROS bridge:
```
source /opt/ros/melodic/setup.bash
source ~/catkin_ws/devel/setup.bash
roslaunch rosbridge_server rosbridge_websocket.launch
```
### Testing the setting by publishing a simple topic:
> **IP setup** -- The IP of two computer need to be set in the same LAN, e.g.:  
1. Computer1 (ROS): 192.168.0.1  
2. Computer2 (Unity): 192.168.0.2  

> **ROS**
1. Checking IP to ensure the IP was set correctly:
```
hostname -I
```
2. Typing following command to publish a example topic(Twist):
```
rostopic pub -r 10 /cmd_vel geometry_msgs/Twist  "{linear:  {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0, z: 0.0}}"
```

> **Unity**
1. Download ROS#
2. ROS connector:  
   > Serializer: Newtonsoft_JSON  
   > Protocal: Web Socket Sharp  
   > ROS bridge Server URL: ws://`192.168.0.1`:9090  
3. Odometry Subscriber: (just take `OdometrySubscriber` as a example)
   > Topic: /cmd_cel   

---
## Run the code (ROS)
#### To compile the file after modified the code:
```
cd catkin_ws 
source /opt/ros/melodic/setup.sh 
rosdep install --from-paths src --ignore-src --rosdistro melodic -y --skip-keys libfranka 
catkin_make -DCMAKE_BUILD_TYPE=Release -DFranka_DIR:PATH=/opt/ros/melodic/lib/libfranka/build 
source devel/setup.sh 
```

#### To source directories:
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

#### Free motion + force field (froce controller)
```
roslaunch franka_panda_controller_swc force_controller_NR.launch robot_ip:=172.16.0.2 load_gripper:=true
```

#### Isometric motion + force field (Cartesian impedance controller)
```
roslaunch franka_panda_controller_swc cartesian_impedance_controller_NR.launch robot_ip:=172.16.0.2 load_gripper:=true
```


## Receive the messages from ROS in Unity
#### Add new message types in Unity
URL:https://github.com/siemens/ros-sharp/wiki/Dev_NewMessageTypes  
> **Try to use **method 1**, which is the easiest method.**  
> **Make sure the PACKAGE name is same as the package name in ROS**  

#### Subscribe the topic from Unity  
The following comparing page shows how to subscribe the topic from Unity:  
URL: https://github.com/iyawx/franka_panda_controller_swc/commit/bfbfaf7c37914718635930921f98d808971f99d4


## Theory
#### Free motion + force field  
> Parameters we got: p (position from end-effector) & dt (time segment: 0.001s)  
> Output parameters: Fp (force to Unity)  
1. v = [vx; vy; vz] = (p_current - p_previous) / dt  
2. Fp = [0 -K K;  K 0 -K; -K K 0] * [vx; vy; vz]
   = A * v  

#### Isometric training + force field  
> Parameters we got: Fs (force from sensor)  
> Output parameters: Fp (force to Unity)  
1. Initialize the initial Fs = 0   
2. Fp = [0 -K K;  K 0 -K; -K K 0] * sum(Fs / m) 
   = A * sum(Fs / m)   


_A = [0 -K K;  K 0 -K; -K K 0]   
m = mass of the object_

## Vidoe
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/79889031/130375329-de7d1102-1ed1-434a-b487-48d265a773d4.gif)


# An integrate version with OpenSim which was built by Joshua Rolls
https://github.com/apatakiparadise/isosim?fbclid=IwAR2cHhlbM-6iOwNr2RWRLKANFDlJUQpKtKOxNBU-4XQK7DdEmV3E7ZGsgFc  
https://github.com/apatakiparadise/isoros?fbclid=IwAR0ofkTSM8KY_IQ1X4uPFFRlIJxl_BQ1DrWB-tjKgDwR6tUZq9GJh8fyIrI  
