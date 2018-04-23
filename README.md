# staubli_barrett_meta_package
Packages and a quick tutorial for bringing up the Staubli arm and Barrett hand on Ubuntu 14.04

## Disclaimer
I have tried many options to get the Barrett hand working on 16.04/ROS Kinetic. Due to incompatibilities from [Peak Linux drivers](http://www.peak-system.com/fileadmin/media/linux/index.htm) and the [pcan_python](https://github.com/RobotnikAutomation/pcan_python) package, I have given up on trying to get it work. In order to get this working with Kinetic (now Melodic because 18.04 was released), someone would have to take the pcan_python package and debug what messages are sent over the PCAN controller for the Barrett differently than they were being sent in 14.04. 

## Purpose
This meta package contains references to other Barrett and Staubli packages which are needed to bringup the arm. Additionally, a package, barrett_staubli_launcher, exists in this workspace which contains the launch files required to bring up the hand, arm, and force torque sensor at the same time. 

## Requirements

- [ROS Indigo](http://wiki.ros.org/indigo)
- [gitman](https://github.com/jacebrowning/gitman)
- [peak-linux-driver-7.15.2.tar.gz](https://www.peak-system.com/fileadmin/media/linux/files/peak-linux-driver-7.15.2.tar.gz)
- [pcan_python](https://github.com/RobotnikAutomation/pcan_python)

## Installation

Install apt requirements
```bash
$ sudo apt update
$ sudo apt install libpopt-dev build-essential ros-indigo-desktop-full swig
```

Install the peak linux driver:
```bash
$ wget https://www.peak-system.com/fileadmin/media/linux/files/peak-linux-driver-7.15.2.tar.gz
$ tar -xvf peak-linux-driver-7.15.2.tar.gz
$ cd peak-linux-driver-7.15.2
$ make -j$(($(nproc) + 1)) NET=NO_NETDEV RT=NO_RT
$ sudo make install
```

Install pcan_python. You'll need to add `export PYTHONPATH=$PYTHONPATH:/usr/lib` to your .bashrc because that is where pcan_python gets installed by default.
```bash
$ git clone https://github.com/RobotnikAutomation/pcan_python.git
$ cd pcan_python
$ sudo make install
$ echo "export PYTHONPATH=$PYTHONPATH:/usr/lib" >> ~/.bashrc
```

Install gitman. On Ubuntu 14.04 you'll need to install python 3.5:
```bash
$ sudo add-apt-repository ppa:fkrull/deadsnakes
$ sudo apt-get update
$ sudo apt-get install python3.5
# Next install pip3 using python3.5
$ wget https://bootstrap.pypa.io/get-pip.py
$ python3.5 get-pip.py --user
# Next install gitman using pip3
$ pip3 install gitman==1.5b1
```

This tutorial assumes you have a workspace already created. In the instructions I will be titled it <catkin_ws>
```bash
$ cd <catkin_ws>/src
$ git clone https://github.com/CRLab/staubli_barrett_meta_package.git
$ cd staubli_barrett_meta_package
$ gitman install
$ cd <catkin_ws>
$ catkin build
```

## Running
In order to bring up the arm and hand together, I created one launch file `staubli_barretthand_bringup.launch` under the `staubli_barretthand_bringup` package.
```bash
$ roslaunch staubli_barretthand_bringup staubli_barretthand_bringup.launch
```
If you want to run the force torque sensor too, run this:
```bash
$ roslaunch staubli_barretthand_bringup staubli_barretthand_bringup_ft.launch
```

To run the MoveIt! planners then run
```bash
$ roslaunch staubli_barretthand_bringup staubli_barretthand_planners.launch
```

## Notes
The configuration for the Staubli arm is set up assuming that the robot is connected on your network at 192.168.1.254. The robot is configured to broadcast a static IP on a local router at this IP address. If you want to change this setting you'll have to reconfigure the robot by going into the settings for the robot. Go to `Control Panel` > `Controller configuration` > `Network` > `J204 IP address` > `Address`. Press the <kbd>↵</kbd> key to edit the field and type out the IP you want to use. The images shown below show my settings for a local IP address. Make sure in your router configuration that you have set the subnet mask to `255.255.255.0` and not reserved `192.168.1.254` by changing the range to `1-253` or lower. 

#### Control Panel
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/control_panel.jpg" />

#### Controller Configuration
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/controller_configuration.jpg" />

#### Network
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/network.jpg" />

#### J204 Config
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/J204.jpg" />

#### IP Settings
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/IP_settings.jpg" />

#### Edit IP Address
<img src="https://raw.githubusercontent.com/CRLab/staubli_barrett_meta_package/master/_images/edit_IP.jpg" />


## Sources
This repo is replacing quite a few old staubli/barrett repos which are now defunct/were disorganized. They have subsequently been archived.
- [https://github.com/CRLab/staubli_barrett_ws](https://github.com/CRLab/staubli_barrett_ws)
- [https://github.com/CRLab/staubli_barretthand_description](https://github.com/CRLab/staubli_barretthand_description)
- [https://github.com/CRLab/staubli_barretthand_moveit_config](https://github.com/CRLab/staubli_barretthand_moveit_config)
