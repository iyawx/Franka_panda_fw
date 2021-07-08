**This page is for NeuroRehack 2021 project 2 Setup**  

# Connecting ROS and Unity
URL:http://wiki.ros.org/rosbridge_suite  

### Download ROS bridge:
```
sudo apt-get install ros-melodic-rosbridge-suite
```
### Connecting the ROS with Unity via ROS bridge:
```
source /opt/ros/melodic/setup.bash
roslaunch rosbridge_server rosbridge_websocket.launch
```


