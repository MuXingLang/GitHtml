# Tomcat

## 1.下载

​		通过tomcat官网的版本归档中查找合适的tomcat版本下载：https://archive.apache.org/dist/tomcat/

​		如下载8.0.9版本tomcat地址为：https://archive.apache.org/dist/tomcat/tomcat-8/v8.0.9/bin/

​		下载完成后直接解压到指定目录即可。

​		想下载该版本Tomat源码则访问：https://archive.apache.org/dist/tomcat/tomcat-8/v8.0.9/src/

## 2.目录结构

### 2.1 bin

​		解压版中bin目录主要是用来存放tomcat的命令，主要有两大类，一类是以.sh结尾的（linux命令），另一类是以.bat结尾的（windows命令），windows安装版中bin目录中可能存在两个exe文件（一般类似于tomcat7.exe和tomcat7w.exe）,用于在控制台或GUI窗口中启动tomcat。

​		很多环境变量的设置都在此处，例如可以设置JDK路径、tomcat路径，其中：

​		startup 用来启动tomcat 
​		shutdown 用来关闭tomcat 
​		catalina可以设置tomcat的内存

### 2.2 conf

​		conf目录主要是用来存放tomcat的一些配置文件

​		`server.xml`配置整个服务器信息，可以设置端口号、设置域名或IP、默认加载的项目、请求编码，添加虚拟主机等等。 
​		`web.xml`为部署描述文件，其中注册了很多MIME类型，即文件类型，通过设置MIME可以设置tomcat支持的文件类型 。
​		`context.xml`对于所有应用的统一配置，可以用来配置数据源之类的。 
​		`tomcat-users.xml`存储tomcat用户的文件，用来配置管理tomcat的用户与权限，即相应的用户名密码以及角色等等，可以按照该文件的注释信息添加tomcat用户。 
​		在Catalina目录下可以设置默认加载的项目 

### 2.3 lib

​		lib目录主要用来存放tomcat运行需要加载的jar包，相当于tomcat的类库。 
​		例如，像连接数据库的jdbc的包我们可以加入到lib目录中来

### 2.4 logs

​		logs目录用来存放tomcat在运行过程中产生的日志文件，非常重要的是在控制台输出的日志。（清空不会对tomcat运行带来影响） 
​		在windows环境中，控制台的输出日志在catalina.xxxx-xx-xx.log文件中 
​		在linux环境中，控制台的输出日志在catalina.out文件中

### 2.5 temp

​		temp目录用户存放tomcat在运行过程中产生的临时文件。（清空不会对tomcat运行带来影响，但最好先停止tomcat再清空该目录） 

### 2.6 webapps

​		webapps目录用来存放应用程序，当tomcat启动时会去加载webapps目录下的应用程序。可以以文件夹、war包、jar包的形式发布应用。 

​		其中ROOT是一个特殊的项目，在地址栏中没有给出项目目录时，对应的就是ROOT项目

​		当然，你也可以把应用程序放置在磁盘的任意位置，在配置文件中映射好就行。

### 2.7 work

​		work目录用来存放tomcat在运行时的编译后文件，例如JSP编译后的文件。 
​		清空work目录，然后重启tomcat，可以达到清除缓存的作用。

### 2.8 声明


参考链接1：https://blog.csdn.net/u012661010/article/details/73381599

参考链接2：https://www.cnblogs.com/zzyytt/p/7697943.html