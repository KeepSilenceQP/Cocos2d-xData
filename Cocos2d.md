Cocos2d-x 阶段总结
=================

![cocos2d-x_logo](http://cocos2d-x.org/images/logo.png "cocos logo")

一、背景
-------

主要用来记录最近对于Cocos2d-x的了解情况，由于接下来会暂停这方面的学习而去完成其他任务，所以以防再次学习时没有线索，就将近几日的学习情况记录下来，方便之后继续学习。

二、环境搭建 及 创建第一个*空*项目
-------------------------------

###Step 1

---

[Win7+Eclipse搭建Cocos2d-x 3.5开发环境][method-1] 我使用该资料在Win7下搭建了Cocos2d-x开发环境。从该文档中可以总结出：

1. 编译使用Python脚本进行；
2. 使用C++开发完毕后，进入Android工程文件夹下，使用Python脚本编译完成即可生成Android工程，导入Eclipse中即可打包APK文件。

之后看到了[Cocos2d-x环境搭建教程汇总](http://cn.cocos2d-x.org/tutorial/lists?id=145)这一系列文章，看到了这篇文章[Cocos2d-x 3.4环境搭建及apk发布调试之谜海归巢][method-2]，不禁疑惑究竟**Cygwin**所起的作用是什么？当然，我知道Cygwin是一个Windows下的Unix-Like环境，提供对于gcc/gdb/make等工具的支持，但是我根据[Win7+Eclipse搭建Cocos2d-x 3.5开发环境][method-1]完成环境搭建后，成功编译并打包了APK，此时我并没有安装配置Cygwin。所以我疑惑Cygwin的作用。再次看过[Cocos2d-x 3.4环境搭建及apk发布调试之谜海归巢][method-2]之后，我认为**对于编译Android项目，Cocos2d-x提供了两种方式，一种是Python脚本，一种是Cygwin+NDK**。在[官方配置文档](http://www.cocos.com/doc/article/index?type=cocos2d-x&url=/doc/cocos-docs-master/manual/framework/native/v3/getting-started/setting-up-development-environments-on-windows7-with-eclipse/zh.md)中也没有发现需要配置Cygwin的地方。

之后，在proj.android文件夹下，看到了**README.md**文件，其中，

> #### Setup Eclipse Environment (only once)
> 
> 
> **NOTE:** This step needs to be done only once to setup the Eclipse environment for cocos2d-x projects. Skip this section if you've done this before.
> 
> 1. Download Eclipse ADT bundle from [Google ADT homepage](http://developer.android.com/sdk/index.html)
> 
>    **OR**
> 
>    Install Eclipse with Java. Add ADT and CDT plugins.
> 
> 2. Only for Windows
>     1. Install [Cygwin](http://www.cygwin.com/) with make (select make package from the list during the install).
>     2. Add `Cygwin\bin` directory to system PATH variable.
>     3. Add this line `none /cygdrive cygdrive binary,noacl,posix=0,user 0 0` to `Cygwin\etc\fstab` file.
>    
> 3. Set up Variables: 
> 	1. Path Variable `COCOS2DX`: 
> 		* Eclipse->Preferences->General->Workspace->**Linked Resources**
> 		* Click **New** button to add a Path Variable `COCOS2DX` pointing to the root cocos2d-x directory.
> 		![Example](https://lh5.googleusercontent.com/-oPpk9kg3e5w/UUOYlq8n7aI/AAAAAAAAsdQ/zLA4eghBH9U/s400/cocos2d-x-eclipse-vars.png)
> 
> 	2. C/C++ Environment Variable `NDK_ROOT`: 
> 		* Eclipse->Preferences->C/C++->Build->**Environment**.
> 		* Click **Add** button and add a new variable `NDK_ROOT` pointing to the root NDK directory.
> 		![Example](https://lh3.googleusercontent.com/-AVcY8IAT0_g/UUOYltoRobI/AAAAAAAAsdM/22D2J9u3sig/s400/cocos2d-x-eclipse-ndk.png)
> 		* Only for Windows: Add new variables **CYGWIN** with value `nodosfilewarning` and **SHELLOPTS** with value `igncr`
> 		
> 4. Import libcocos2dx library project:
> 	1. File->New->Project->Android Project From Existing Code.
> 	2. Click **Browse** button and open `cocos2d-x/cocos2dx/platform/android/java` directory.
> 	3. Click **Finish** to add project.
> 	
> #### Adding and running from Eclipse
> 
> ![Example](https://lh3.googleusercontent.com/-SLBOu6e3QbE/UUOcOXYaGqI/AAAAAAAAsdo/tYBY2SylOSM/s288/cocos2d-x-eclipse-project-from-code.png) ![Import](https://lh5.googleusercontent.com/-XzC9Pn65USc/UUOcOTAwizI/AAAAAAAAsdk/4b6YM-oim9Y/s400/cocos2d-x-eclipse-import-project.png)
> 
> 1. File->New->Project->Android Project From Existing Code
> 2. **Browse** to your project directory. eg: `cocos2d-x/cocos2dx/samples/Cpp/TestCpp/proj.android/`
> 3. Add the project 
> 4. Click **Run** or **Debug** to compile C++ followed by Java and to run on connected device or emulator.
> 

从以上引用中可以看到，Cygwin的主要作用应该就是支持直接在Eclipse中进行编译运行。但是，我没有操作过，所以不方便下结论。

对于以上问题，留下一个可供解决问题的伏笔 -- **Android NDK开发**学习。有理由相信，通过学习Android NDK开发，可以明白上面进行如此配置的原因。

[method-1]: http://cn.cocos2d-x.org/tutorial/show?id=2777
[method-2]: http://cn.cocos2d-x.org/tutorial/show?id=2645

###Step 2

---

+ Cocos2d-x中新建一个项目有两种不同的方式，分别是

    1. 使用cocos.py脚本；
    2. 使用cocos命令。

cmd下通过使用where命令，发现脚本和命令文件均位于安装位置cocos2d-x-3.7\tools\cocos2d-console\bin中。

+ 对于Android平台上程序的生成，关键是借助NDK开发，其中我所感兴趣的是**基于Cocos2d-x所生成的项目进行二次开发，把Android平台的UI等内容应用于程序中**。目前我不知道是否可行，料想应该是应用Android所提供的NDK技术，这一方面需要留下伏笔，有待日后补充。下图是我浏览Cocos2d-x所生成的Android项目的结构，所发现的应该与多平台支持实现所相关的一系列库文件，

+ 下图是我浏览搭建教程时发现的不错的对于Android.mk文件的配置信息，截图如下，

三、开发入门教程
--------------

[Cocos2d-x](http://cn.cocos2d-x.org/tutorial/index?type=cocos2d-x)提供了许多教程来指导开发者，我大概浏览过这些教程之后选择出以下内容，

1. [Cocos2dx-制作FlappyBird](http://blog.csdn.net/column/details/flappybird.html) 通过所提供的教程里找到了作者自己的CSDN博客，这是[项目代码地址](https://github.com/OiteBoys/Earlybird)。我已经把这个项目在机器上搭建好了，如果不是临时有事情，就计划通过这个项目入门的。该项目的搭建十分容易，把源码解压后，直接将其对应版本**[cocos2d-x-3.0beta2](http://cdn.cocos2d-x.org/cocos2d-x-3.0beta2.zip)**下载下来，解压改名为**cocos2d**放到项目根文件夹下即可。
2. [【Cocos2d-x v3.2】分析经典游戏FlappyBird各个类对象源码](http://cn.cocos2d-x.org/tutorial/lists?id=88) 一个初学者根据上面的资料写的自己的代码，并自己把自己的代码分析了一遍。建议参考一下即可。
3. [Cocos2d-x 制作2048](http://cn.cocos2d-x.org/tutorial/lists?id=6) 不解释。
4. [《2048》手游开发揭秘视频教程](http://cn.cocos2d-x.org/tutorial/lists?id=58) 也不解释。
5. [Cocos2d-x 3.x学习笔记：猩先生带你打飞机](http://cn.cocos2d-x.org/tutorial/lists?id=140) 手把手教你写一个打飞机的游戏。
6. 如果上面的打飞机你感觉不过瘾，还有这个：[【cocos2d-x入门实战】微信飞机大战讲解](http://blog.csdn.net/column/details/jackyairplane.html)。
7. [Cocos2d-x 3.3塔防游戏《保卫女孩》从零开始](http://cn.cocos2d-x.org/tutorial/lists?id=131) 塔防游戏。
8. [Cocos2d-x 3.2编写常用UI组件](http://cn.cocos2d-x.org/tutorial/lists?id=128) 刚入门或许用不到，但是之后肯定会有用的。
9. [Cocos2d-x开发基础知识汇总](http://cn.cocos2d-x.org/tutorial/lists?id=91) 各种基础，现用现查。
10. [玩转Cocos2d-x](http://cn.cocos2d-x.org/tutorial/lists?id=15) 现查现用。
11. [【系列视频教程】Cocos2d-x游戏开发基础与实战](http://cn.cocos2d-x.org/tutorial/lists?id=86) 这是一套视频教程，看视频比较慢，建议看看前几讲，对Cocos2d-x有一个总体认识，然后可以寻找自己关心的再看。
12. [学习使用Cocos制作《闹钟》](http://cn.cocos2d-x.org/tutorial/show?id=2438) 通过这个可以学习如何使用Cocos进行开发。

我打算再继续先看，

[Cocos2d-x概要（更新中）](http://www.maiziedu.com/course/cocos2d-x/117-972/)

[Cocos2d-x功能扩展-C++/Cocos2d-x/Android/iOS混合编程与NDK开发环境搭建](http://www.jikexueyuan.com/course/62.html)

四、视频
-------

1. [2048游戏-Cocos2d-x游戏实战项目开发](http://www.jikexueyuan.com/course/48.html)
2. [Cocos2d-x游戏实战项目开发-Flappy Bird游戏](http://www.jikexueyuan.com/course/34.html)
3. [Cocos2d-x游戏开发开发知识体系图](http://www.jikexueyuan.com/path/cocos2d-x/)
4. [Cocos2d-x手游开发](http://www.maiziedu.com/course/cocos2d-x/)

五、其他
-------

###[Cocos2d-x GitHub](https://github.com/cocos2d/cocos2d-x)

注意ReadMe.md。

###[cocos2d-x-samples GitHub](https://github.com/cocos2d/cocos2d-x-samples)

建议先运行一遍进行学习。

###[Cocos2d 总站](http://www.cocos2d.org/)

一系列Cocos2d的项目。

###[cocos官网](http://www.cocos.com/)

中文

###Cocos2d-x 中英文手册

[英文手册][english-manual]与[中文手册][chinese-manual]还是有不同之处的。初步感觉[英文手册][english-manual]是从更基础的游戏概念讲起，比如对于精灵等概念也会详细说明，而[中文手册][chinese-manual]讲解较为抽象。但是，[中文手册][chinese-manual]内容更全面，也便于阅读。

[english-manual]: http://cocos2d-x.org/programmersguide/
[chinese-manual]: http://www.cocos.com/doc/article

###[中文论坛](http://www.cocoachina.com/bbs/thread.php?fid=41)

###[如何学习 cocos2d-x ？](http://zengrong.net/post/2100.htm)

参考一下即可，明白Cocos2d-x开发涉及到的东西。

###[cocos2d-android](https://code.google.com/p/cocos2d-android/)

已经凋零，留以纪念。
