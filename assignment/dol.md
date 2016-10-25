Lab3:DOL实例分析&编程
=====
* 实验目的
* 实验过程
* 实验结果
* 实验感想

## 实验目的

  * 修改example1，使其输出3次方数
  * 修改example2，让3个square模块变成2个

## 实验过程

  * 分析代码
    * Src文件夹中的文件定义了各个模块的功能，包括生产者、消费者、过程处理模块(如example2中为平方模块)
    * XML文件则定义了各个模块间的连接方式
    * .c文件
      * 进程定义：包括xxx_init()和xxx_fire()，其中，xxx_init()为初始化函数，只执行一次，而xxx_fire则可能被执行无数次
      * 端口输出：`DOL_write((void*)PORT_OUT, &(x), sizeof(float), p);`将x写到当前进程的“PORT_OUT”端口
      * 端口读取：`DOL_read((void*)PORT_IN, &c, sizeof(float), p);`从当前进程的“PORT_IN”端口读取数据
    * XML文件
      * 进程定义(dot图中的框)：
        * 主体`<process>`标签，必须包含的子标签为`<port>`,`<source>`,形式如下：
        * `<port type=“type” name=“name”/>`type为input和output两种
        * `<source type=“c” location=“location.c"/>` location.c为对应定义进程行为的C文件
      * 通道定义(dot图中的线)：
        * 主体为`<sw_channel type=“fifo” size=“size” name=“name"></sw_channel>`size为缓冲区大小，name为这条线的名字
        * 通道有两个`<port>`定义端口，一个为input，一个为output
      * 连接的实现
        * connection标签中包含两个子标签：`<origin>``<target>`，分别为该连线的指出方和被指向方，但每个子标签必须通过`<port>`标签表明通过哪个端口进行连接，如下：
        ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDolmd/connection.png)
      * 迭代器
        * `<iterator variable="i" range="N">`表明在这个迭代器中迭代的值为i，i从0迭代到N,共迭代了N次，实验中通过迭代实现循环

  * 代码修改
    * example1
      * 由代码分析我们知道在square.c中定义了平方操作，所以要改为三次方只需要将其中的`i = i*i;`改为`i = i*i*i;`即可

    * example2
      * example2中的次数由iterator定义，原始赋值为N = 3，因而要修改为2个square模块只需要将N的大小改为2

## 实验结果
  * example1
    ![example1Result](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDolmd/example1.JPG)

  * example2
    ![example2Result](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDolmd/example2.JPG)
    ![example2Dot](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDolmd/example2dot.JPG)

## 实验感想
  * 明白了之前example1中的框图的生成过程和数值的计算过程：使用进程间的通信来实现计算的过程，然后在XML文件中，对应到相应的进程，得到对应的执行框图
  * 明白了DOL下多进程的实现方式和不同进程间通信的实现：通过DOL_write()和DOL_read()实现对端口数据的写与读
  * 学会了XML文件一些简单的元素：process, port, iterator, connection...明白了这些元素和实验的几个进程的对应关系，明白了他们的基本使用
