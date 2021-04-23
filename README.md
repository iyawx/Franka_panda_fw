# Setup

# Franka-Emika Panda Setup

Linux: Unbuntu 18.04 LTS (Bionic Beaver) 

ROS: melodic 

Real-time kernel: 5.4.40-rt24 (may change) 
 

 

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

> **libfranka**

URL: https://frankaemika.github.io/docs/installation_linux.html

```
sudo apt install ros-melodic-libfranka ros-melodic-franka-ros 
sudo apt remove "*libfranka*" 
sudo apt install build-essential cmake git libpoco-dev libeigen3-dev 
git clone --recursive https://github.com/frankaemika/libfranka 
cd libfranka 
mkdir build 
cd build 
cmake -DCMAKE_BUILD_TYPE=Release .. 
cmake --build . 
```
 

> **franka_ros** 

URL: https://frankaemika.github.io/docs/installation_linux.html 

```
mkdir -p catkin_ws/src 
cd catkin_ws 
source /opt/ros/melodic/setup.sh 
catkin_init_workspace src 
git clone --recursive https://github.com/frankaemika/franka_ros src/franka_ros 
rosdep install --from-paths src --ignore-src --rosdistro melodic -y --skip-keys libfranka 
catkin_make -DCMAKE_BUILD_TYPE=Release -DFranka_DIR:PATH=/home/<user name>[1]/libfranka/build 
source devel/setup.sh 
```
 

_[1] You can find the path of dependencies by using “pwd” command._

 


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
sudo usermod -a -G realtime $<user_name> 
```

Open the limits file: 
```
sudo -H gedit /etc/security/limits.conf 
```

Add the following limits to limits file: 

> @realtime soft rtprio 99 
>
> @realtime soft priority 99 
>
> @realtime soft memlock 102400 
>
> @realtime hard rtprio 99 
>
> @realtime hard priority 99 
>
> @realtime hard memlock 102400 
