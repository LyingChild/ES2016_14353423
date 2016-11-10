ROS安装
====
在不同版本的ubuntu虚拟机上装了不同版本的ROS，但是最后的cartographer都不能正常装完，所以放弃了，这份报告以在ubutu 14.04下安装ROS indigo为例

* 往更新源添加ROS indigo的源地址: 
  * `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'` 

* 建立key
  * `sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116`

* 获取更新
  * `sudo apt-get update`
* 下载ROS源码
  * `sudo apt-get install ros-indigo-desktop-full`
  * 成功下载结果如下：

  ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForROS/install-ros-indigo-desktop-full.JPG)

* 初始化rosdep
  * `sudo rosdep init`

  ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForROS/sudo-rosdep-init.JPG)

* 检查更新rosdep
  * `rosdep update`

  ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForROS/rosdep-update.JPG)

* 配置ROS环境变量
  * `echo "source /opt/ros/indigo/setup.bash" >> ~/.bash`
  * `source ~/.bash`

* 安装rosinstall（用于获取ROS包的源码树）
  * `sudo apt-get install python-rosinstall`
  
  ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForROS/install-python-rosinstall.JPG)

* 写在最后
  * 本来想把cartographer做完，写报告也好写些，但是既然一直没法正常装cartographer也就算了，就写查找资料过程学到的一些东西吧
    1. ROS(Robot Operating System)是一个灵活的架构，开发用于编写机器人软件，旨在通过一个广泛的多样的机器人平台来简化创建复杂的健壮的机器人行为。ROS中包含了很多工具、类库或者约定，有这样一个等式：ROS=管道+工具集+性能+生态系统
    2. Cartographer则是谷歌开源同步定位与制图库，主要是基于激光雷达的SLAM实现