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
roslaunch rosbridge_server rosbridge_websocket.launch
```
### Testing the setting by publishing a simple topic:
> IP setup
The IP of two computer need to be set as same LAN, e.g.:  
  Computer1 (ROS): 192.168.0.1  
  Computer2 (Unity): 192.168.0.2  

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

#### joint impedance controller
```
roslaunch franka_panda_controller_swc joint_impedance_controller.launch robot_ip:=172.16.0.2 load_gripper:=true
```
#### froce controller
```
roslaunch franka_panda_controller_swc force_controller_NR.launch robot_ip:=172.16.0.2 load_gripper:=true
```

## Receive the messages from ROS
#### Add new message types
URL:https://github.com/siemens/ros-sharp/wiki/Dev_NewMessageTypes  
**Try to use **method 1**, which is the easiest method.**  
**Make sure the PACKAGE name is same as the package name in ROS**  

