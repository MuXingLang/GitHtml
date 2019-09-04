# Java基础

## 1. 概述

​		Java是一门面向对象编程语言，不仅吸收了C++语言的各种优点，还摒弃了C++里难以理解的多继承、指针等概念，因此Java语言具有功能强大和简单易用两个特征。Java语言作为静态面向对象编程语言的代表，极好地实现了面向对象理论，允许程序员以优雅的思维方式进行复杂的编程  。
​		Java具有简单性、面向对象、分布式、健壮性、安全性、平台独立与可移植性、多线程、动态性等特点 。Java可以编写桌面应用程序、Web应用程序、分布式系统和嵌入式系统应用程序等 。

## 2. JDK

​		JDK是 Java 语言的软件开发工具包，主要用于移动设备、嵌入式设备上的java应用程序。JDK是整个java开发的核心，它包含了JAVA的运行环境（JVM+Java系统类库）和JAVA工具。

​		目前推荐使用JDK8.0以上的版本，以下为下载链接，可选取对应版本下载

```
https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```

​		Windows下安装JDK一般需要配置环境变量，网上教程很多，配置项大致如下：

```
JAVA_HOME	//新建
	//对应JDK安装目录
Path	//新增或编辑
	;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin; 
CLASSPATH	//新建
	.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```

​		配置完成后打开命令行输入如下命令可查看版本

```
>java -version
java version "1.8.0_191"
Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)
```

## 3. Eclipse

​		Eclipse 是一个开放源代码的、基于Java的可扩展开发平台。就其本身而言，它只是一个框架和一组服务，用于通过插件组件构建开发环境。

​		Eclipse是著名的跨平台的自由集成开发环境（IDE）。最初主要用来Java语言开发，通过安装不同的插件Eclipse可以支持不同的计算机语言，比如C++和Python等开发工具。Eclipse的本身只是一个框架平台，但是众多插件的支持使得Eclipse拥有其他功能相对固定的IDE软件很难具有的灵活性。许多软件开发商以Eclipse为框架开发自己的IDE。

​		Eclipse 最初由OTI和IBM两家公司的IDE产品开发组创建，起始于1999年4月。IBM提供了最初的Eclipse代码基础，包括Platform、JDT 和PDE。Eclipse项目IBM发起，围绕着Eclipse项目已经发展成为了一个庞大的Eclipse联盟，有150多家软件公司参与到Eclipse项目中。

​		Eclipse是一个开放源码项目，它其实是Visual Age for Java的替代品，其界面跟先前的Visual Age for Java差不多，但由于其开放源码，任何人都可以免费得到，并可以在此基础上开发各自的插件，因此越来越受人们关注。随后还有包括Oracle在内的许多大公司也纷纷加入了该项目，Eclipse的目标是成为可进行任何语言开发的IDE集成者，使用者只需下载各种语言的插件即可。

## 4. IDEA

​		IDEA 全称 IntelliJ IDEA，是java编程语言开发的集成环境。IntelliJ在业界被公认为最好的java开发工具之一，尤其在智能代码助手、代码自动提示、重构、J2EE支持、各类版本工具(git、svn等)、JUnit、CVS整合、代码分析、 创新的GUI设计等方面的功能可以说是超常的。

​		IDEA是JetBrains公司的产品，这家公司总部位于捷克共和国的首都布拉格，开发人员以严谨著称的东欧程序员为主。它的旗舰版本还支持HTML，CSS，PHP，MySQL，Python等。免费版只支持Python等少数语言。