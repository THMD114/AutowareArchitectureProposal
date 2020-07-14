# Autoware (Architecture Proposal) for Arm project 

The content of this page is a preliminary version.

# What's this

BASE:https://github.com/tier4/AutowareArchitectureProposal
This version is adjusted for the ARM processor (A64) test environment. 
Please do not use the base setup file "setup_ubuntu18.04.sh" as it is an installer for x86. 

# How to setup

## Requirements

### Environment

 - A64 CPU 
 - 16 GB or more of memory
 - Ubuntu server 18.04 LTS & SD Card 
   reference: (https://community.arm.com/developer/tools-software/oss-platforms/w/docs/457/n1sdp-getting-started-guide)

※If cuda or tensorRT is already installed, it is recommended to remove it.

## Autoware setup

1. Clone this repository

```
git clone https://github.com/THMD114/AutowareArchitectureProposal
cd AutowareArchitectureProposal/
```

2. Install next softwares 

   In this step, the following software are installed.
   Please confirm their licenses before using them.

- [osqp](https://github.com/oxfordcontrol/osqp/blob/master/LICENSE)

  ```
  git clone --recursive https://github.com/oxfordcontrol/osqp
  cd osqp/
  mkdir build 
  cd build/
  cmake -G "Unix Makefiles" ..
  cmake --build . --target install
  ```

- [ROS Melodic](https://github.com/ros/ros/blob/noetic-devel/LICENSE)
  http://wiki.ros.org/melodic/Installation/Ubuntu

- [CUDA 11.0 for arm64](https://docs.nvidia.com/cuda/eula/index.html)
  https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=sbsa&compilation=compilation_native&target_distro=Ubuntu&target_version=1804&target_type=deblocal

- [cuDNN 7](https://docs.nvidia.com/deeplearning/sdk/cudnn-sla/index.html)

- [TensorRT 7](https://docs.nvidia.com/deeplearning/sdk/tensorrt-sla/index.html)

- [geographiclib-tools](https://geographiclib.sourceforge.io/html/LICENSE.txt)

3. Other Settings

- Rosdep installation is required. If installation fails, set avahi to disable by following the procedure below. 

  ```
  rosdep init
  rosdep update
  systemctl disable avahi-daemon.socket
  systemctl disable avahi-daemon.service 
  rosdep install -i --from-paths src
  ```

- Roswww melodic version is required. 

  ```
  apt-get install ros-melodic-roswww 
  ```

- If you cannot start due to an undefined localhost error, add it according to the attachment procedure.

  ```
  sh -c 'echo 127.0.1.1 localhost >> /etc/hosts'
  ```

- Build the source

  ```
  catkin build --cmake-args -DCMAKE_BUILD_TYPE=Release
  ```

# How to run

### Quick Start

#### Rosbag

1. Download sample map from [here](https://drive.google.com/open?id=1ovrJcFS5CZ2H51D8xVWNtEvj_oiXW-zk) and extract the zip file.
2. Download sample rosbag from [here](https://drive.google.com/open?id=1BFcNjIBUVKwupPByATYczv2X4qZtdAeD).
3. Launch Autoware

```
source devel/setup.bash
roslaunch autoware_launch autoware.launch map_path:=[path] rosbag:=true
```

　And you can see that the ros node is working.
　(Example: rostopic echo /[node name])

