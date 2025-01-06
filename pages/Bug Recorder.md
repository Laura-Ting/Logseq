## Technical Tips:
- ### VScode
- How to search files and directories by name in VSCode? Ctrl+P
- ### X11 Forwarding
- MobaXterm use ssh to connnect may solve the problems of X11 rather than using local connection, normally the steps are as shown in https://blog.csdn.net/weixin_50973728/article/details/129882910
- `sudo /etc/init.d/ssh restart`  or  `sudo service ssh restart`
- ### CMake and Anaconda
- sometimes, `catkin` will find the version of Python in the environment of Anaconda
- e.g.
- ```
  CMake Error at /opt/ros/noetic/share/catkin/cmake/empy.cmake:30 (message):
    Unable to find either executable 'empy' or Python module 'em'...  try
    installing the package 'python3-empy'
  ```
- we should specify the certain path
- ```
  catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
  ```
-
- ## 未解之谜
- 1. ubuntu运行rviz报错：QXcbConnection: XCB error
- ```
  sudo apt-get --reinstall install libqt5dbus5 libqt5widgets5 libqt5network5 libqt5gui5 libqt5core5a libxcb-xinerama0
  sudo apt-get install libqt5x11extras5
  sudo apt-get install --reinstall libxcb-xinerama0
  ```
- Clio会出现
- 上述安装方法仍未解决
-
- ## 配Clio时候踩的坑
-
- 先保证安装上ros neotic
- The catkin CMake module was not found, but it is required to build a linked workspace. 这个问题可能是ros未安装导致
-
- install GTSAM
- issue
- ```
  CMake Error at /root/catkin_ws/src/kimera_rpgo/CMakeLists.txt:17 (find_package):
  By not providing "FindGTSAM.cmake" in CMAKE_MODULE_PATH this project has asked CMake to find a 
  package configuration file provided by "GTSAM", but CMake did not find one. Could not find a 
  package configuration file provided by "GTSAM" with any of the following names: GTSAMConfig.cmake
  gtsam-config.cmake Add the installation prefix of "GTSAM" to CMAKE_PREFIX_PATH or set "GTSAM_DIR"
  to a directory containing one of the above files. If "GTSAM" provides a separate development 
  package or SDK, be sure it has been installed.
  ```
-
- first install the dependency
- [Boost](http://www.boost.org/users/download/) >= 1.65, [CMake](http://www.cmake.org/cmake/resources/software.html) >= 3.0, gcc 4.7.3 on Linux, TBB
- ```
  sudo apt-get install libboost-all-dev
  sudo apt-get install cmake
  sudo apt-get install libtbb-dev
  ```
- ubuntu20.04 must select the version GTSAM >= 4.0.3 otherwise an error “The "debug" argument must be followed by a library.” might occur
- the version 4.2.0 can be obtained through: https://github.com/borglab/gtsam/releases/tag/4.2.0
- ```
  #!bash
  $ cd gtsam-1.X.X #change to your own
  $ mkdir build
  $ cd build
  $ cmake ..
  $ make check
  $ sudo make install
  ```
-
-
- Python virtualenv
- install on Ubuntu
- ```
  sudo apt install python3-virtualenv # for python3
  ```
- create an environment
- ```
  virtualenv myenv
  virtualenv -p /usr/bin/python3.7 myenv
  ```
- activate & deactivate an environment
- ```
  source myenv/bin/activate
  deactivate
  ```