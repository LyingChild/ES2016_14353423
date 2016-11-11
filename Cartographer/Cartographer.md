Cartographer安装&&体验
===
ROS indigo在Ubuntu14.04上的安装详见上一份实验报告，这部分为cartographer在ROS indigo中的安装流程

* ### 安装过程
* ### 体验
* ### 感想

## Cartographer安装(路径自定)
  * 安装依赖性：`sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev python-wstool python-rosdep`,一次性将所有依赖的包全部安装，避免后面的步骤出现缺少依赖包的错误，在此之前需添加ROS的源(如果之前没添加的话)。** 其他ROS版本需将 ros-indigo-tf2-eigen 包换成对应版本的包，例如：ROS jade换成ros-jade-tf2-eigen **
  ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/依赖项.JPG)

  * 安装ceres solver
    1. 从GitHub下载ceres solver项目:`git clone https://github.com/hitcm/ceres-solver-1.11.0.git`
    2. 进入ceres-solver-1.11.0目录并创建build文件夹并进入：
    ```
       cd ceres-solver-1.11.0
       mkdir build
       cd build
    ```
    3. 检测编译环境，并生成相应的makefile: `cmake ..`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/cmake.JPG)
    4. 编译：`make`。保险起见单线编译，追求速度的话可以开启多线编译，不过开启过多线程可能导致宕机，方法：`make -jn` n为线程数
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/make.png)
    5. 安装：`sudo make install`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/sudo-make-install.png)

  * 安装 cartographer(路径自定)
    1. 下载cartographer：`git clone https://github.com/hitcm/cartographer.git`
    2. 同样到文件夹内新建build文件夹并进入：
    ```
       cd cartographer
       mkdir build
       cd build
    ```
    3. 检测编译环境，并指定生成的makefile为Ninja: `cmake .. -G Ninja`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/cmake-G-Ninja.png)
    4. 编译：`ninja`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/ninja.png)
    5. 检测编译后的完整性：`ninja test`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/ninja-test.png)
    6. 安装：`sudo ninja install`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/sudo-ninja-install.png)

    * Tip: 相比于安装ceres solver中使用的make来进行编译，ninja的编译速度更快。也可以将此部分的3-6步换成
    ```
       cmake ..
       make 
       sudo make install
    ```

  * 安装cartographer_ros(路径自定)
    1. 新建catkin_ws目录并初始化工作空间
    ```
       mkdir catkin_ws
       cd catkin_ws
       wstool init src
       cd src
    ```
    2. 下载到catkin_ws: `git clone https://github.com/hitcm/cartographer_ros.git`
    3. 通过rosdep解决依赖：`rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y`
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/rosdep解决依赖.JPG)
      * tip: 出错的话可以试着执行`echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc`,`source ~/.bashrc`
    4. 安装：
    ```
       cd ..
       catkin_make
    ```
    到此，cartographer_ros安装成功
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/catkin_make.png)
    
## 下载数据并体验
  * 下载数据
    2d数据，大概500M，用迅雷下载
    https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag
    3d数据，8G左右，同样用迅雷下载
	https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/cartographer_3d_deutsches_museum.bag
  
  * 体验(只跑了2d的数据包)
    1. 把下载好的包放到虚拟机随意目录下(为了方便放在Download)
    2. 运行`roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag`
    3. 结果：
       可能是内存不足或者其他问题，每次都不能完整的跑完，只能得到一部分的地图，下面是最好的一次
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForCartographer/部分.png)

## 感想
  1. 完成了cartographer的配置，并体验了用cartographer结合ROS模拟机器人绘制地图的过程
  2. 在写实验报告的过程了解了很多语句的作用和区别
  3. cartographer ROS模拟是个极耗CPU又极耗内存的过程，每一步更新地图相隔的时间都是挺长的，而且越到后面越长，然后很容易因为其他事情就停下来不干活了orz，最好的那次是更新了148次地图，得到上面的结果。