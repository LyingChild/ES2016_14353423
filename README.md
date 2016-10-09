DOL框架
================
  * 分布式操作层(DOL)是一种使应用程序能自动或半自动地映射到多处理器平台的一种框架,基本上由三部分组成:
     * DOL应用程序编程接口(API)
     * DOL功能仿真
     * DOL映射优化

## DOL开发环境配置

  * 安装ANT和java环境
  * 编译systemc
  * 编译dol
  * 运行示例

### 具体过程
  * **安装ANT和Java环境**<br/>
  由于虚拟机中的ubuntu为32位，所以没有用提供的64位JDK，而是在线安装java环境。<br/>另：由于此部分均为在线安装，过程较简单，所以没有截图也不会外加描述
  * 更新软件包信息：`sudo apt-get update`
     * 安装ANT包：`sudo apt-get install ant`
     * 安装JDK：`sudo apt-get install openjdk-7-jdk`
  * **编译systemc**
   	 * 将systemc-2.3.1.tgz包拷到虚拟机中并解压(图形界面)
   	 * 进入systemc-2.3.1文件夹：`cd systemc-2.3.1`
   	 * 新建文件夹objdir：`mkdir objdir`
   	 * 进入文件夹：`cd objdir`
   	 * 运行configure：`../configure CXX=g++ --disable-async-updates` ![运行configure](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image_for_readme/configure.png)
   	 * 编译systemc：`sudo make install` ![编译systemc](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image_for_readme/make_systemc.png)
  * **编译dol**
     * 在home目录新建dol文件夹，将dol_ethz.zip解压到里面
       * `mkdir dol`
       * `unzip dol_ethz.zip -d dol`
     * 修改build_zip.xml文件中systemc的位置 ![环境变量](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image_for_readme/build_zip.JPG)
     * 编译dol：`ant -f build_zip.xml all` ![buildSucc](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image_for_readme/ant_f_build_zip.png)
  * **运行示例**
     * 进入mian目录并运行示例
        * `cd build/bin/main`
        * `ant -f runexample.xml -Dnumber=1` ![test](https://github.com/LyingChild/ES2016_14353423/blob/master/Image_for_readme/%E7%BC%96%E8%AF%91dol.png?raw=true)
     * 结果如下：<br>
    ![dot](https://github.com/LyingChild/ES2016_14353423/blob/master/Image_for_readme/%E6%8D%95%E8%8E%B7.JPG?raw=true)
    
### 实验感想
  * 在安装环境的时候没出什么大问题,只是因为之前把虚拟机设置成中文导致build过程过不去,把系统改回英语就解决了,安装过程更需要的是发现问题的方法,这次错误提示在中文的那行,然后想到大部分开发包都是不支持中文的,试了一下成功解决
  * 学会了markdown的基本语句,相较之下,md没有那么多的规则,写起来也随意.舒服很多,不过也因为语言本身就实现了很多,有时候会遇到只了解表层没法解决的问题
  * 初步学会了使用git来实现版本管理,并用github托管
