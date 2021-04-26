<div class="text-orange-light mb-2">
# Franka-Emika Panda Setup

**Linux: Unbuntu 18.04 LTS (Bionic Beaver)** 

**ROS: melodic** 

**Real-time kernel: 5.4.40-rt24 (may change)**
</div>

 ---

# 1. Linux

### 1.1 Partition 40-50GB of hard disk from the original OS 

Windows: https://support.microsoft.com/en-us/windows/create-and-format-a-hard-disk-partition-bbb8e185-1bda-ecd1-3465-c9728f7d7d2e 

Apple: https://support.apple.com/en-au/guide/disk-utility/dskua9e6a110/mac 

 

### 1.2 Download Ubuntu 18.04 

Download `Ubuntu 18.04 desktop image (64-bit PC (AMD64) desktop image)` via the following URL: https://releases.ubuntu.com/18.04/ 

 

### 1.3 Download Rufus 

Preparing a USB drive and download Rufus from the following URL: https://rufus.ie/en/ 

Plugin the USB drive > Run Rufus > (In Rufus) > Device: `your USB drive` > Boot selection: `Ubuntu iso file` (the file you download in the last step) > Click `Start` button > Turn off your computer after the procedure is finished. 

 

### 1.4 Install Ubuntu 18.04 

Turn on your computer and get into the `BOOT` menu (Different brand of computers have different boot keys, please refer to: https://www.desertcrystal.com/bootkeys).  

Select your USB drive > Install Ubuntu > Select your language > Select the keyboard you are using > Normal installation > Erase disk and install Ubuntu (ensure it is installed in the right partition. You can check the partition setup via “Something else”) > Select your time zone > Input your name and password > Install the Ubuntu and reboot your computer after it finished 

---
 

# 2. ROS

### 2.1 Download ROS melodic 

Turn on the terminal and input the following commands: 

URL: http://wiki.ros.org/melodic/Installation/Ubuntu

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' 
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 
sudo apt update 
sudo apt install ros-melodic-desktop-full 
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc 
source ~/.bashrc 
source /opt/ros/melodic/setup.bash 
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential 
sudo apt install python-rosdep 
sudo rosdep init 
rosdep update 
```
 

### 2.2 Install libfranka and franka_ros 

To install libfranka and franka_ros, input the following commands in the terminal. 
```
sudo apt install ros-melodic-libfranka ros-melodic-franka-ros 
```
If you run `sudo apt install ros-melodic-libfranka ros-melodic-franka-ros` command, you do not need to run following command.

> **libfranka**

URL: https://frankaemika.github.io/docs/installation_linux.html

```
sudo apt remove "*libfranka*"
cd /opt/ros/melodic/lib
sudo apt install build-essential cmake git libpoco-dev libeigen3-dev 
sudo git clone --recursive https://github.com/frankaemika/libfranka 
cd libfranka 
mkdir build 
sudo cd build 
sudo cmake -DCMAKE_BUILD_TYPE=Release .. 
sudo cmake --build . 
```
 

> **franka_ros** 

URL: https://frankaemika.github.io/docs/installation_linux.html 

```
cd
mkdir -p catkin_ws/src 
cd catkin_ws 
source /opt/ros/melodic/setup.sh 
catkin_init_workspace src 
git clone --recursive https://github.com/frankaemika/franka_ros src/franka_ros 
rosdep install --from-paths src --ignore-src --rosdistro melodic -y --skip-keys libfranka 
catkin_make -DCMAKE_BUILD_TYPE=Release -DFranka_DIR:PATH=/opt/ros/melodic/lib/libfranka/build 
source devel/setup.sh 
```
 

_[1] You can find the path of dependencies by using “pwd” command._

 
---

# 3. Real-time kernel

### 3.1 Kernel checking 

`uname -a`: Check which kernel is currently being used. 
`uname -r`: Check the version of kernel. 

 

### 3.2 install real-time kernel 

URL: https://frankaemika.github.io/docs/installation_linux.html 

```
sudo apt-get install build-essential bc curl ca-certificates fakeroot gnupg2 libssl-dev lsb-release libelf-dev bison flex 
uname -r 
```

uname -r will output the version of current kernel. The version of kernel may change, please refer to https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/ and find a version that is reasonably close to your current version (slightly lower will be preferable). For example, my current kernel version is `5.4.0-42`. Thus, I chose `5.4.40` kernel version with `5.4.40-rt24 patch`. The following instructions are exemplary for `5.4.40` kernel version. 

```
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.40.tar.xz 
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.40.tar.sign 
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/5.4/older/patch-5.4.40-rt24.patch.xz 
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/5.4/older/patch-5.4.40-rt24.patch.sign 
xz -d linux-5.4.40.tar.xz 
xz -d patch-5.4.40-rt24.patch.xz 
gpg2 --verify linux-5.4.40.tar.sign 
gpg2  --keyserver hkp://keys.gnupg.net --recv-keys 0x<key_ID>[2]
gpg2 --verify patch-5.4.40-rt24.patch.sign 
gpg2  --keyserver hkp://keys.gnupg.net --recv-keys 0x<key_ID>[2] 
```

_[2] two key ID will be shown by `gpg2 --verify linux-5.4.40.tar.sign` and `gpg2 --verify patch-5.4.40-rt24.patch.sign` commands_


After the keys has been set up, the kernel will be compiled by following commands:  
```
tar xf linux-5.4.40.tar 
cd linux-5.4.40
patch -p1 < ../patch-5.4.40-rt24.patch 
make oldconfig 
```

After make oldconfig command, the terminal will open a configuration menu. When asked for the `Preemption model`, choose the `Fully Preemptible Kernel`, and other options at their default values. Then turn on **System monitor (Computer application)** and check the number of your CPU cores. Input the following commands: 
```
make -j<number of CPU cores> deb-pkg 
sudo dpkg -i ../linux-headers-5.4.40-rt24_*.deb ../linux-image-5.4.40-rt24_*.deb 
```
### 3.3 Verify new kernel
These commands may take 30 minutes to several hours (depends on your CPU). After the process has been done, turn off the Ubuntu. Then, turn on the ubuntu and get into boot menu. Go to `Advanced option for Ubuntu` and then select `Linux 5.4.40-rt24`. Once the ubuntu was fully started, turn on terminal and type following command: 
```
uname -a 
```

The output must contain the string `PREEMPT RT` and the version number. Next, check the following file which should contain the number `1` 
```
sudo -H gedit /sys/kernel/realtime 
```

Further, add a group named realtime and the user controlling your robot to this group. 
```
sudo addgroup realtime 
sudo usermod -a -G realtime $(whoami) 
```

Open the limits file: 
```
sudo -H gedit /etc/security/limits.conf 
```

Add the following limits to limits file: 
```
@realtime soft rtprio 99 
@realtime soft priority 99 
@realtime soft memlock 102400 
@realtime hard rtprio 99 
@realtime hard priority 99 
@realtime hard memlock 102400 
```
Reboot you computer again. 

---

# 4. Ethernet connection
Please refer to: https://frankaemika.github.io/docs/getting_started.html
### 4.1 Workstation
Coonect to franka master controller
* **OS:** Ubuntu 18.04
* **Ethernet 1:** IPv4 Setting > `address`: 172.16.0.1, `Netmask`: 24
* **PCI Ethernet:** Wifi via USB port

### 4.2 Laptop
Connect to franka robot port X5
* **OS:** Window 10
* **Ethernet:** Default

### 4.3 Connection
> **On your Laptop:**
1. Open your internet browser (e.g.`Chrome`, `Firefox` or `Microsoft Edge`), access DESK via `https://robot.franka.de`
2. Setup your user name and password > Shop floor network: `172.16.0.2` > end-effector: `None` > FrankaWorld: you can register you robot if you have franka world account.
3. Now, you should see a franka user interface. You can test the robot by applying `APP` to check the connection between laptop and robot is fine (remember to unlock you robot before you run the app). 
4. Check the `Setting` > `System` > `Installed Features`, you should see `Franka Control Interface (FCI)`. 
* If you have FCI, you need to connect your end-effector before you go to the next step. (`Setting` > `End-effector` > `Franka Hand` > `Apply`) 
* If not, you need to register your robot with franka world. If you already registered, please contact `support@franka.de`. 

> **On the workstation:**
5. Open terminal, check the Ubuntu run in the real-time kernel
```
uname -a
```
The output must contain the string `PREEMPT RT` and the version number.

6. Source ROS before you run the examples:
```
source /opt/ros/melodic/setup.sh
```
The directory of ROS might change, depends on your setup.

7. Change to the build directory of `libfranka` and execute the example:
```
cd /opt/ros/melodic/lib/libfranka
./echo_robot_state 172.16.0.2
```
* If you are success, that means your connection between workstation and robot is fine. 
* If not, please refer to: https://frankaemika.github.io/docs/troubleshooting.html and have a look at `Robot is not reachable` section.

8. Change to the build directory of `franka_ros` and execute the example:
```
cd
cd /opt/ros/melodic/include
roslaunch franka_example_controllers joint_impedance_example_controller.launch robot_ip:=172.16.0.2 load_gripper:=true
```
9. Download ros controller (some franka_ros example controllers will have errors due to `position_joint_trajectory_controller`)
```
sudo apt-get install ros*controller*
```
---

# 5. Moveit!
### 5.1 Download Moveit!
You HAVE TO download Moveit! since most of the frnaka_ros example controller have real-time simulation. URL:http://docs.ros.org/en/melodic/api/moveit_tutorials/html/doc/getting_started/getting_started.html
```
cd
rosdep update
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install ros-melodic-catkin python-catkin-tools
sudo apt install ros-melodic-moveit
mkdir -p ~/ws_moveit/src
cd ~/ws_moveit/src
git clone https://github.com/ros-planning/moveit_tutorials.git -b melodic-devel
git clone https://github.com/ros-planning/panda_moveit_config.git -b melodic-devel
cd ~/ws_moveit/src
rosdep install -y --from-paths . --ignore-src --rosdistro melodic
cd ~/ws_moveit
catkin config --extend /opt/ros/${ROS_DISTRO} --cmake-args -DCMAKE_BUILD_TYPE=Release -DFranka_DIR:PATH=/opt/ros/melodic/lib/libfranka/build
catkin build
source ~/ws_moveit/devel/setup.bash
```

### 5.2 Test franka_ros and Moveit
```
cd
cd catkin_ws
roslaunch franka_example_controllers move_to_start.launch robot_ip:=172.16.0.2 load_gripper:=true
```
---
# 6. rqt_plot
rqt_plot can plot and record the robot configuration which is a useful tool for testing

### 6.1 Download rqt_plot
```
sudo apt-get install ros-melodic-rqt
sudo apt-get install ros-melodic-rqt-common-plugins
```
### 6.2 Run rqt_plot
First, execute the launch file and open another terminal. Then, type following command:
```
rqt_plot
```
Type in the `Topic` input field the full path of the topic name, and press `+` button. You can check what topics you have by using `rostopic list` command.

---

# Appendix
To source directories:
```
source /opt/ros/melodic/setup.sh
source ./catkin_ws/devel/setup.bash
source ~/ws_moveit/devel/setup.bash
```

