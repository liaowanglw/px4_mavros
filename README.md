# px4_mavros  
实现功能：无人机在Gazebo仿真环境下，解锁并起飞到2米高度。  

---  

## 一 环境  
主机环境：ubuntu 18.04 LTS  
ROS版本：melodic  
飞控：Pixhawk 1  

---  
## 二 前置工作  
* 1 在ubuntu操作系统上安装好ROS，并完成初始化、环境变量和工作空间的配置。    
* 2 下载好PX4源码：https://github.com/PX4/Firmware  

---  

## 三 安装MAVROS包
`sudo apt install ros-melodic-mavros ros-melodic-mavros-extras`  
`wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh`  
`./install_geographiclib_datasets.sh`  

---  
## 四 源码编译  
`cd ~/catkin_ws/src`  
`git clone https://github.com/mavlink/mavros.git`  

`cd ..`  
`catkin_make`  

---  
## 五 安装依赖包  
`sudo apt install ros-melodic-mavlink`  
`sudo apt install ros-melodic-geometry`  
`sudo apt install ros-melodic-control-toolbox`  
`sudo apt install libgeographic-dev`  
`sudo apt install geographiclib-tools`  
  
`cd ~/catkin_ws/src/mavros/mavros/scripts/`  
`sudo ./install_geographiclib_datasets.sh`  

---  
## 六  编写offboard程序  
* 1 建立offboard程序包  
`cd ~/catkin_ws/src`  
`catkin_create_pkg offb roscpp mavros geometery_msgs`  

* 2 编写offboard程序  
`cd ~/catkin_ws/src/offb/src`  
`vim offb_node.cpp`  

复制本例程中的src/offb_node.cpp代码。  

* 3 修改CmakeLists  
`cd ~/catkin_ws/src/offb`  
`vim CmakeLists.txt`  

```
include_directories(  
#include  
  ${catkin_INCLUDE_DIRS}  
)  
  
add_executable(offb_node src/offb_node.cpp)  

target_link_libraries(offb_node  
  $(catkin_LIBRARIES}  
)  
```  
  
---  
## 七 编译  
* 1 编译  
`cd ~/catkin_ws`  
`catkin_make`  

---  
## 八 仿真测试  
* 1 终端1：启动gazebo仿真环境  
`cd ~/src/Firmware`    
`make posix_sitl_default gazebo`  
  
* 2 终端2：运行mavros  
`roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"`  
  
* 3 终端3：启动offboard控制程序  
`source ~/catkin_ws/devel/setup.bash`  
`rosrun offb offb_node`
