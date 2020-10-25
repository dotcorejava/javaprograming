# 第1章 开始Java之旅

## Java 版本
- Java SE(Java Standard Edition)包含了标准的JDK、开发工具、运行环境和类库,适合开发桌面应用程序和底层应用程序,同时也是Java EE的基础平台。
- Java EE(Java Enterprise Edition)采用标准化的模块组件为企业级应用提供了标准平台,简化了复杂的企业级编程。现在Java EE已经成为了一种软件架构和企业级开发的设计思想。
- Java ME(Java Micro Edition)包含高度优化精简的Java运行环境,主要用于开发具有有限的连接、内存和用户界面能力的设备应用程序,如移动电话(手机)、PDA(电子商务)、能够接入电缆服务的机顶盒或各种终端和其他消费电子产品。

## Java 特性
1. 简单  
Java在设计上与C++十分相近。Java中删除了许多极少被使用、不容易理解和令人混淆的C++功能,如运算符重载、多重继承等,增加了内存垃圾自动收集功能,关于内存的分配与释放是使C与C++应用程序变得复杂的常见原因之一。因为Java的垃圾自动收集功能简化了程序设计工作,所以无论是经验丰富的C++/C程序员还是程序设计的初学者,学习Java都是非常容易的。
2. 面向对象  
Java语言以面向对象为基础。在Java语言中,不能在类外面定义单独的数据和函数,所有的对象都要派生于同一个基类,并共享其所有的功能。也就是说,Java语言最外部的数据类型是对象,所有的元素都要通过类和对象来访问。
3. 分布式  
由于Java中内置了TCP/IP、HTTP、FTP等协议,因此Java应用程序可以通过URL地址访问网络上的对象,访问方式与访问本地文件系统几乎完全相同。
4. 解释器通用性  
Java解释器能直接对Java字节码进行解释执行。经过编译生成的字节码可以在提供Java虚拟机的任何一个系统上解释运行,不需要额外存储。
5. 健壮  
Java能够检查程序在编译和运行时的错误。类型检查能帮助用户检查出许多在开发早期出现的错误,同时许多集成开发环境(IDE)的出现使编译和运行Java程序更加容易。
6. 安全  
因为Java的设计目标是提供使用于网络/分布式运算环境,所以安全性问题自然是不容忽视的。Java的验证技术是以公钥加密法为基础的。
7. 可移植性  
Java程序具有与体系结构无关的特性。这一特性使Java程序可以方便地移植到网络上不同的机器。同时Java的类库中也实现了针对不同平台的接口,使这些类库可以移植。
8. 高效能  
虽然Java字节码是解释运行,但是经过仔细设计的字节码可以通过JIT技术转换为高效能的本机代码。
9. 多线程  
Java支持多线程编程。Java运行时,系统在多线程同步方面具有成熟的解决方案。这使得程序设计者将更多的精力关注于程序实现的细节。


## Java环境搭建
- JDK（Java Development Kit）：Java开发工具包，包含JRE和开发Java程序所需的工具,如编译器、调试器、反编译器、文档生成器等。
- JRE（Java Runtime Environment）：Java运行环境，包含类库和JVM。

### 安装JDK
JDK 下载地址：http://java.sun.com/javase/downloads/index.html
```
sudo apt install openjdk-8-jdk

java --version
openjdk 14.0.1 2020-04-14
OpenJDK Runtime Environment (build 14.0.1+7-Ubuntu-1ubuntu1)
OpenJDK 64-Bit Server VM (build 14.0.1+7-Ubuntu-1ubuntu1, mixed mode, sharing) 

javac --version
javac 14.0.1
```

配置环境变量:  
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64  
classpath=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=$JAVA_HOME/bin:$PATH


## Java代码示例
```
mkdir demo
cd demo
touch HelloWorld.java

echo -e 'public class HelloWorld{
        public static void main(String args[]){
                System.out.println("Hello DM!");
        }
}' > HelloWorld.java

javac HelloWorld.java
java HelloWorld
```

## 练习
1. 列举Java的三个版本。
2. 简述Java程序的开发过程。
3. 简述安装JDK需要配置哪些系统变量。
4. 简述Java程序的运行原理。
5. 简述Eclipse编写Java程序的流程。