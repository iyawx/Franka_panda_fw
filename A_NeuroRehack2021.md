**This page is for NeuroRehack 2021 project 2 Setup**  

# Connecting ROS and Unity
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
> **ROS**
1. Checking IP:
```
hostname -I
```
2. Typing following command to publish a topic(Twist):
```
rostopic pub -r 10 /cmd_vel geometry_msgs/Twist  "{linear:  {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0, z: 0.0}}"
```
> **Unity**
ROS#



