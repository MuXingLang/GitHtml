# Nginx

## 1. 源代码安装

​		服务器环境为Center OS 7 64位，使用`yum`命令安装软件。

### 1.1 安装依赖

​		本例中只是安装一些编译Nginx时经常使用的编译环境和软件等，如有需要，可以自定义所需内容。

#### 1.1.1 yum逐条安装版

​		安装gcc和g++两个编译器，可以分开安装，不过要先安装gcc才能安装g++

```
yum -y install gcc gcc-c++ 
```

​		安装make，用于编译

```
yum -y install gcc automake autoconf libtool make
```

​		安装pcre，用于重写rewrite

```
yum -y install pcre
```

​		安装pcre-devel		

```
yum -y install pcre-devel 
```

​		安装zlip，用于开启gzip压缩

```
yum -y install zlip 
```

​		安装zlib-devel

```
yum -y install zlib-devel
```

​		安装openssl

```
yum -y install openssl 
```

​		安装openssl-devel

```
yum -y install  
```

> **Tips**：参数`-y`用于自动选择版本，如需特定版本，可不添加该参数，直接使用`yum install`命令安装，`yum`会在找到安装包之后再由用户手动选择版本。

> **Notes**:devel 包主要是供开发用，至少包括头文件和链接库，有的还含有开发文档或演示代码。编译的时候如果需要用到这个库，那么需要安装这个库的devel，因为需要头文件

#### 1.1.2 yum打包安装版

​		可按照如下所示，将要安装的软件依次以空格间隔拼在后边即可。

```bash
[root@localhost ~]# yum -y install gcc gcc-c++ automake pcre pcre-devel zlip zlib-devel openssl openssl-devel 
```

#### 1.1.3 源码安装PCRE库

​		可从`ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ `下载最新的 PCRE 源码包，使用下面命令下载编译和安装 PCRE 包：

​		切换特定目录

```
cd /usr/local/src
```

​		下载源码包

```
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz 
```

​		解压源码包

```
tar -zxvf pcre-8.37.tar.gz
```

​		切换得到解压目录

```
cd pcre-8.34
```

​		进行安装前配置和检测

```
./configure
```

​		编译

```
make
```

​		安装

```
make install
```

#### 1.1.4 源码安装zlib库

 		可通过`http://zlib.net/zlib-1.2.8.tar.gz` 下载最新的 zlib 源码包，使用下面命令下载编译和安装 zlib包：

```
cd /usr/local/src

wget http://zlib.net/zlib-1.2.8.tar.gz
tar -zxvf zlib-1.2.8.tar.gz
cd zlib-1.2.8
./configure
make
make install
```

#### 1.15 安装ssl

```
cd /usr/local/src
wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
tar -zxvf openssl-1.0.1t.tar.gz
```

### 1.2 下载

​		Nginx 一般有两个版本，分别是稳定版和开发版，您可以根据您的目的来选择这两个版本的其中一个，使用`wget`命令下载相应版本的nginx源码压缩包

```
wget http://nginx.org/download/nginx-1.8.1.tar.gz
```

​		使用`tar`命令解压该源码包

```
tar -xzf nginx-1.8.1.tar.gz
```

> **说明**：nginx在编译时可指定安装路径参数，因此压缩包下载位置和软件安装位置没有必然联系，既可以在需安装目录直接下载，也可在特定下载目录下载后在指定安装路径参数。

### 1.3 内置模块

#### 1.3.1 默认启用的模块

以下参数允许您禁用默认情况下启用的模块：
–without-http_charset_module
–without-http_gzip_module
–without-http_ssi_module
–without-http_userid_module
–without-http_access_module
–without-http_access_module
–without-http_autoindex_module
–without-http_geo_module
–without-http_map_module
–without-http_referer_module
–without-http_rewrite_module
–without-http_proxy_module
–without-http_fastcgi_module
–without-http_uwsgi_module
–without-http_scgi_module
–without-http_memcached_module
–without-http_limit_conn_module
–without-http_limit_req_module
–without-http_empty_gif_module
–without-http_browser_module
–without-http_upstream_ip_hash_module
–without-http_upstream_least_conn_module
–without-http_split_clients_module

#### 1.3.2 默认禁用的模块

以下参数允许您启用默认禁用的模块：
–with-http_ssl_module
–with-http_realip_module
–with-http_addition_module
–with-http_xslt_module
–with-http_image_filter_module
–with-http_geoip_module
–with-http_sub_module
–with-http_dav_module
–with-http_flv_module
–with-http_mp4_module
–with-http_gzip_static_module
–with-http_random_index_module
–with-http_secure_link_module
–with-http_stub_status_module
–with-google_perftools_module
–with-http_degradation_module
–with-http_perl_module
–with-http_spdy_module
–with-http_gunzip_module
–with-http_auth_request_module

### 1.4 自定义配置

​		编译软件之前可以先指定一些配置，以下先看一个官方例子，多个配置项如果换行可使用` \`（空格和反斜杠）分割，也可不加反斜杠，仅用空格分割：

```
./configure --sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-http_ssl_module \
--with-pcre=/opt/app/openet/oetal1/chenhe/pcre-8.37 \
--with-zlib=/opt/app/openet/oetal1/chenhe/zlib-1.2.8 \
--with-openssl=/opt/app/openet/oetal1/chenhe/openssl-1.0.1t
```

其中：

​		`--with-pcre=/usr/src/pcre-8.34` 指的是pcre-8.34 的源码路径。
​		`--with-zlib=/usr/src/zlib-1.2.7` 指的是zlib-1.2.7 的源码路径。

#### 1.3.1 指定安装目录

```
# 其中path即为指定的安装路径
--prefix=path
```

#### 1.3.2 指定可执行文件名

```
# 一般为文件名为prefix/sbin/nginx
--sbin-path=path
```

#### 1.3.3 指定配置文件名

```
# 如果需要，nginx总是可以通过在命令行参数-c文件中指定一个不同的配置文件来启动。
# 默认情况下，文件名为prefix/conf/nginx.conf
--conf-path=path
```

#### 1.3.4 指定pid文件

```
# pid文件，它将存储主进程的进程ID。
# 安装之后，总是可以使用pid指令在nginx.conf配置文件中更改文件名。
# 默认情况下，文件名为prefix/logs/nginx.pid。
--pid-path=path
```

#### 1.3.5 指定用户

```
# 安装之后，可以使用user指令在nginx.conf配置文件中更改名称。
# 默认的用户名是nobody。
--user=name
```

#### 1.3.6 指定组

```
# 安装之后，可以使用user指令在nginx.conf配置文件中更改名称。
# 默认情况下，组名设置为无特权用户的名称。
--group=name
```

#### 1.3.7 开启线程池

```
--with-threads
```

#### 1.3.8 启用https

```
# 默认情况不支持https
# 启用该项需要安装ssl
--with-http_ssl_module
```

#### 1.3.9 启用gzip

```
# 默认不开启此功能
# 开启需要安装zlib
-with-http_gzip_static_module
```

#### 1.3.10 自定义模块目录

```
# 定义将安装nginx动态模块的目录。
# 默认情况下使用prefix/modules目录。
--modules-path=path
```

#### 1.3.11 启用或禁用模块

```
# 启用或禁用构建允许服务器使用的模块。
# 如果平台不支持更合适的方法，如kqueue、epoll或/dev/poll，则自动构建此模块。
# 其中select_module即为模块名
--with-select_module
--without-select_module
```

#### 1.3.12 添加其他模块

```
# 可以添加第三方功能模块
--add-module=path
```

#### 1.3.13 指定C编译器

```
# sets the name of the C compiler. 
--with-cc=path 
```

#### 1.3.14 设置CFLAGS参数。

```
# 设置将添加到CFLAGS变量的其他参数。
--with-cc-opt=parameters
```

#### 1.3.15 指定CPU

```
# 支持为每个指定CPU构建:
# pentium、pentiumpro、pentium3、pentium4、athlon、opteron、sparc32、sparc64、ppc64。
--with-cpu-opt=cpu
```

#### 1.3.16 指定日志路径

```
# 设置主请求的HTTP服务器的日志文件的名称。
# 安装完成后，可以随时在nginx.conf配置文件中更改access_log。
# 默认情况下，文件名 为prefix/logs/access.log.
--http-log-path=path
```

#### 1.3.17 指定错误日志路径

```
# 设置主错误，警告，和诊断文件的名称。
# 安装完成后，可以随时改变nginx.conf配置文件中error_log。
# 默认情况下，文件名 为prefix/logs/error.log.
--error-log-path=path
```

#### 1.3.18 一个例子

```
./configure --prefix=/server/nginx --sbin-path=/server/nginx/sbin/nginx --with-http_stub_status_module --with-pcre --with-http_gzip_static_module --with-http_ssl_module
```

### 1.5 编译

​		执行`make`命令即可编译，如果模块多可有的等。一般命令行界面会持续闪屏一段时间。

### 1.6 安装

​		执行`make install`即可安装,安装路径如文件列表下：

```bash
[root@localhost nginx]# ls
conf  html  logs  nginx-1.8.1  nginx-1.8.1.tar.gz  sbin
```

### 1.7 常用操作

​		使用安装路径`prefix/sbin/nginx`启动

```
/server/nginx/sbin/nginx 
```

​		指定配置文件启动

```
/server/nginx/sbin/nginx -c conf/nginx.conf
```

​		重新启动

```
/server/nginx/sbin/nginx -s reload
```

​		关闭

```
/server/nginx/sbin/nginx -s stop
```

```
pkill -9 nginx
```

​		查看版本

```
/server/nginx/sbin/nginx -V
```

### 1.8 验证

​		nginx默认配置文件占用80端口，运行前需要确认80端口是否被占用，或更改端口号，运行成功后。在命令行下可使用`curl`来测试nginx是否运行成功，成功会返回一个HTML文档。

```
[root@localhost nginx]# curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

​		查看nginx版本信息如下

```
[root@localhost nginx]# /server/nginx/sbin/nginx -V
nginx version: nginx/1.8.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/server/nginx --sbin-path=/server/nginx/sbin/nginx --with-http_stub_status_module --with-pcre --with-http_gzip_static_module --with-http_ssl_module
```

## 2.配置文件详解

​		nginx配置文件由多个块组成，最外面的块是main，main包含Events和HTTP，HTTP包含upstream和多个Server，Server又包含多个location：

​		main（全局设置）、server（主机设置）、upstream（负载均衡服务器设置）和 location（URL匹配特定位置的设置）。

- main块设置的指令将影响其他所有设置；

- server块的指令主要用于指定主机和端口；

- upstream指令主要用于负载均衡，设置一系列的后端服务器；

- location块用于匹配网页位置。

​		这四者之间的关系式：server继承main，location继承server，upstream既不会继承其他设置也不会被继承。
​		在这四个部分当中，每个部分都包含若干指令，这些指令主要包含Nginx的主模块指令、事件模块指令、HTTP核心模块指令，同时每个部分还可以使用其他HTTP模块指令，例如Http SSL模块、HttpGzip Static模块和Http Addition模块等。

### 2.1 全局配置

示例代码如下：

```
user nobody nobody;
worker_processes 2;
error_log logs/error.log notice;
pid logs/nginx.pid;
worker_rlimit_nofile 65535;
 
events{
use epoll;
worker_connections 65536;
}
```

​		每个配置选项的含义解释如下：

- user是个主模块指令，指定Nginx Worker进程运行用户以及用户组，默认由nobody账号运行。
- worker_processes是个主模块指令，指定了Nginx要开启的进程数。每个Nginx进程平均耗费10M~12M内存。建议指定和CPU的数量一致即可。
- error_log是个主模块指令，用来定义全局错误日志文件。日志输出级别有debug、info、notice、warn、error、crit可供选择，其中，debug输出日志最为最详细，而crit输出日志最少。
- pid是个主模块指令，用来指定进程pid的存储文件位置。
- worker_rlimit_nofile用于绑定worker进程和CPU， Linux内核2.4以上可用。

a). events事件指令是设定Nginx的工作模式及连接数上限：
b). use是个事件模块指令，用来指定Nginx的工作模式。Nginx支持的工作模式有select、poll、kqueue、epoll、rtsig和/dev/poll。其中select和poll都是标准的工作模式，kqueue和epoll是高效的工作模式，不同的是epoll用在Linux平台上，而kqueue用在BSD系统中。对于Linux系统，**epoll工作模式是首选。**
c). worker_connections也是个事件模块指令，用于定义Nginx每个进程的最大连接数，默认是1024。最大客户端连接数由worker_processes和worker_connections决定，即Max_client=worker_processes*worker_connections。
在作为反向代理时，max_clients变为：max_clients = worker_processes * worker_connections/4。
进程的最大连接数受Linux系统进程的最大打开文件数限制，在执行操作系统命令“ulimit -n 65536”后worker_connections的设置才能生效

### 2.2 HTTP服务器配置

```
http{
http{
include conf/mime.types;
default_type application/octet-stream;
log_format main '$remote_addr - $remote_user [$time_local] '
'"$request" $status $bytes_sent '
'"$http_referer" "$http_user_agent" '
'"$gzip_ratio"';
log_format download '$remote_addr - $remote_user [$time_local] '
'"$request" $status $bytes_sent '
'"$http_referer" "$http_user_agent" '
'"$http_range" "$sent_http_content_range"';
client_max_body_size 20m;
client_header_buffer_size 32K;
large_client_header_buffers 4 32k;
Sendfile on;
tcp_nopush on;
tcp_nodelay on;
keepalive_timeout 60;
client_header_timeout 10;
client_body_timeout 10;
send_timeout 10;
```

​		下面详细介绍下这段代码中每个配置选项的含义。
-  include是个主模块指令，实现对配置文件所包含的文件的设定，可以减少主配置文件的复杂度。类似于Apache中的include方法。
- default_type属于HTTP核心模块指令，这里设定默认类型为二进制流，也就是当文件类型未定义时使用这种方式，例如在没有配置PHP环境时，Nginx是不予解析的，此时，用浏览器访问PHP文件就会出现下载窗口。

下面的代码实现对日志格式的设定：

```
log_format main '$remote_addr - $remote_user [$time_local] '
'"$request" $status $bytes_sent '
'"$http_referer" "$http_user_agent" '
'"$gzip_ratio"';
log_format download '$remote_addr - $remote_user [$time_local] '
'"$request" $status $bytes_sent '
'"$http_referer" "$http_user_agent" '
'"$http_range" "$sent_http_content_range"';
```

​		log_format是Nginx的HttpLog模块指令，用于指定Nginx日志的输出格式。main为此日志输出格式的名称，可以在下面的access_log指令中引用。

- client_max_body_size用来设置允许客户端请求的最大的单个文件字节数；
- client_header_buffer_size用于指定来自客户端请求头的headerbuffer大小。对于大多数请求，1K的缓冲区大小已经足够，如果自定义了消息头或有更大的Cookie，可以增加缓冲区大小。这里设置为32K；
- large_client_header_buffers用来指定客户端请求中较大的消息头的缓存最大数量和大小， “4”为个数，“128K”为大小，最大缓存量为4个128K；
- sendfile参数用于开启高效文件传输模式。将tcp_nopush和tcp_nodelay两个指令设置为on用于防止网络阻塞；
- keepalive_timeout设置客户端连接保持活动的超时时间。在超过这个时间之后，服务器会关闭该连接；
- client_header_timeout设置客户端请求头读取超时时间。如果超过这个时间，客户端还没有发送任何数据，Nginx将返回“Request time out（408）”错误；
- client_body_timeout设置客户端请求主体读取超时时间。如果超过这个时间，客户端还没有发送任何数据，Nginx将返回“Request time out（408）”错误，默认值是60；
- send_timeout指定响应客户端的超时时间。这个超时仅限于两个连接活动之间的时间，如果超过这个时间，客户端没有任何活动，Nginx将会关闭连接。

### 2.3 HttpGzip模块配置

​		下面配置Nginx的HttpGzip模块。这个模块支持在线实时压缩输出数据流。
​		看是否安装了HttpGzip模块：

```
[root@vps ~]# /opt/nginx/sbin/nginx  -V
nginx version: nginx/1.0.14
built by gcc 4.4.6 20110731 (Red Hat 4.4.6-3) (GCC)
configure arguments: --with-http_stub_status_module --with-http_gzip_static_module --prefix=/opt/nginx
```

​		通过`/opt/nginx/sbin/nginx -V`命令可以查看安装Nginx时的编译选项，由输出可知，我们已经安装了HttpGzip模块。

​		下面是HttpGzip模块在Nginx配置中的相关属性设置：

```
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_http_version 1.1;
gzip_comp_level 2;
gzip_types text/plain application/x-javascript text/css application/xml;
gzip_vary on;
```

- gzip用于设置开启或者关闭gzip模块，“gzip on”表示开启GZIP压缩，实时压缩输出数据流；
- gzip_min_length设置允许压缩的页面最小字节数，页面字节数从header头的Content-Length中获取。默认值是0，不管页面多大都进行压缩。建议设置成大于1K的字节数，小于1K可能会越压越大；
- gzip_buffers表示申请4个单位为16K的内存作为压缩结果流缓存，默认值是申请与原始数据大小相同的内存空间来存储gzip压缩结果；
- gzip_http_version用于设置识别HTTP协议版本，默认是1.1，目前大部分浏览器已经支持GZIP解压，使用默认即可；
- gzip_comp_level用来指定GZIP压缩比，1 压缩比最小，处理速度最快；9 压缩比最大，传输速度快，但处理最慢，也比较消耗cpu资源；
- gzip_types用来指定压缩的类型，无论是否指定，“text/html”类型总是会被压缩的；
- gzip_vary选项可以让前端的缓存服务器缓存经过GZIP压缩的页面，例如用Squid缓存经过Nginx压缩的数据。

### 2.4 负载均衡配置

​		下面设定负载均衡的服务器列表：

```
upstream cszhi.com{
ip_hash;
server 192.168.8.11:80;
server 192.168.8.12:80 down;
server 192.168.8.13:8009 max_fails=3 fail_timeout=20s;
server 192.168.8.146:8080;
}
```

​		upstream是Nginx的HTTP Upstream模块，这个模块通过一个简单的调度算法来实现客户端IP到后端服务器的负载均衡。
​		在上面的设定中，通过upstream指令指定了一个负载均衡器的名称cszhi.com。这个名称可以任意指定，在后面需要的地方直接调用即可。

​		Nginx的负载均衡模块目前支持4种调度算法，下面进行分别介绍，其中后两项属于第三方的调度方法。

- 轮询（默认）：每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某台服务器宕机，故障系统被自动剔除，使用户访问不受影响；
- Weight：指定轮询权值，Weight值越大，分配到的访问机率越高，主要用于后端每个服务器性能不均的情况下；
- ip_hash：每个请求按访问IP的hash结果分配，这样来自同一个IP的访客固定访问一个后端服务器，有效解决了动态网页存在的session共享问题；
- fair：比上面两个更加智能的负载均衡算法。此种算法可以依据页面大小和加载时间长短智能地进行负载均衡，也就是根据后端服务器的响应时间来分配请求，响应时间短的优先分配。Nginx本身是不支持fair的，如果需要使用这种调度算法，必须下载Nginx的upstream_fair模块；
- url_hash：按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率。Nginx本身是不支持url_hash的，如果需要使用这种调度算法，必须安装Nginx 的hash软件包。

​		在HTTP Upstream模块中，可以通过server指令指定后端服务器的IP地址和端口，同时还可以设定每个后端服务器在负载均衡调度中的状态。常用的状态有：

- down：表示当前的server暂时不参与负载均衡；
- backup：预留的备份机器。当其他所有的非backup机器出现故障或者忙的时候，才会请求backup机器，因此这台机器的压力最轻；
- max_fails：允许请求失败的次数，默认为1。当超过最大次数时，返回proxy_next_upstream 模块定义的错误；
- fail_timeout：在经历了max_fails次失败后，暂停服务的时间。max_fails可以和fail_timeout一起使用。

​		注意，当负载调度算法为ip_hash时，后端服务器在负载均衡调度中的状态不能是weight和backup。

### 2.5 server虚拟主机配置

​		下面介绍对虚拟主机的配置。
​		建议将对虚拟主机进行配置的内容写进另外一个文件，然后通过include指令包含进来，这样更便于维护和管理。

```

server{
listen 80;
server_name 192.168.8.18 cszhi.com;
index index.html index.htm index.php;
root /wwwroot/www.cszhi.com
charset gb2312;
access_log logs/www.ixdba.net.access.log main;
}
```

​		server标志定义虚拟主机开始，listen用于指定虚拟主机的服务端口，server_name用来指定IP地址或者域名，多个域名之间用空格分 开。index用于设定访问的默认首页地址，root指令用于指定虚拟主机的网页根目录，这个目录可以是相对路径，也可以是绝对路径。Charset用于 设置网页的默认编码格式。access_log用来指定此虚拟主机的访问日志存放路径，最后的main用于指定访问日志的输出格式。

### 2.6 location URL匹配配置

​		URL地址匹配是进行Nginx配置中最灵活的部分。 location支持正则表达式匹配，也支持条件判断匹配，用户可以通过location指令实现Nginx对动、静态网页进行过滤处理。使用location URL匹配配置还可以实现反向代理，用于实现PHP动态解析或者负载负载均衡。
​		以下这段设置是通过location指令来对网页URL进行分析处理，所有扩展名			
​		以.gif、.jpg、.jpeg、.png、.bmp、.swf结尾的静态文件都交给nginx处理，而expires用来指定静态文件的过期时间，这里是30天。

```
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
root /wwwroot/www.cszhi.com;
expires 30d;
}
```

​		以下这段设置是将upload和html下的所有文件都交给nginx来处理，当然，upload和html目录包含在`/web/wwwroot/www.cszhi.com`目录中。

```
location ~ ^/(upload|html)/ {
root /web/wwwroot/www.cszhi.com;
expires 30d;
}
```

​		在最后这段设置中，location是对此虚拟主机下动态网页的过滤处理，也就是将所有以.jsp为后缀的文件都交给本机的8080端口处理。

```
location ~ .*.php$ {
index index.php;
proxy_pass http://localhost:8080;
}
```

### 2.7 StubStatus模块配置

​		StubStatus模块能够获取Nginx自上次启动以来的工作状态，此模块非核心模块，需要在Nginx编译安装时手工指定才能使用此功能。
​		以下指令实指定启用获取Nginx工作状态的功能。

```
location /NginxStatus {
stub_status on;
access_log logs/NginxStatus.log;
auth_basic "NginxStatus";
auth_basic_user_file ../htpasswd;
}
```

​		stub_status设置为“on”表示启用StubStatus的工作状态统计功能。access_log 用来指定StubStatus模块的访问日志文件。auth_basic是Nginx的一种认证机制。auth_basic_user_file用来指定认证的密码文件，由于Nginx的auth_basic认证采用的是与Apache兼容的密码文件，因此需要用Apache的htpasswd命令来生成密码文件，例如要添加一个test用户，可以使用下面方式生成密码文件：

```
/usr/local/apache/bin/htpasswd -c  /opt/nginx/conf/htpasswd test
```

​		然后输入两次密码后确认之后添加用户成功。

​		要查看Nginx的运行状态，可以输入`http://ip/NginxStatus`，输入创建的用户名和密码就可以看到Nginx的运行状态：

```
Active connections: 1
server accepts handled requests
34561 35731 354399
Reading: 0 Writing: 3 Waiting: 0
```

​		Active connections表示当前活跃的连接数，第三行的三个数字表示 Nginx当前总共处理了34561个连接， 成功创建次握手， 总共处理了354399个请求。最后一行的Reading表示Nginx读取到客户端Header信息数， Writing表示Nginx返回给客户端的Header信息数
，“Waiting”表示Nginx已经处理完，正在等候下一次请求指令时的驻留连接数。

​		在最后这段设置中，设置了虚拟主机的错误信息返回页面，通过error_page指令可以定制各种错误信息的返回页面。在默认情况下，Nginx会在主目录的html目录中查找指定的返回页面，特别需要注意的是，这些错误信息的返回页面大小一定要超过512K，否者会被ie浏览器替换为ie默认的错误页面。

```
error_page 404 /404.html;
error_page 500 502 503 504 /50x.html;
location = /50x.html {
root html;
}
}
```

### 2.8 网络实例

原文链接：https://www.cnblogs.com/xiaogangqq123/archive/2011/03/02/1969006.html

#### 2.8.1 基本配置

```
#运行用户
user www-data;   
#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志及PID文件
error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

#工作模式及连接数上限
events {
    use   epoll;             #epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以上内核,可以大大提高nginx的性能
    worker_connections  1024;#单个后台worker process进程的最大并发链接数
    # multi_accept on;
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
     #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，
    #必须设为 on,如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #tcp_nopush     on;

    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    tcp_nodelay        on;
   
    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";

    #设定请求缓冲
    client_header_buffer_size    1k;
    large_client_header_buffers  4 4k;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

    #设定负载均衡的服务器列表
     upstream mysvr {
    #weigth参数表示权值，权值越高被分配到的几率越大
    #本机上的Squid开启3128端口
    server 192.168.8.1:3128 weight=5;
    server 192.168.8.2:80  weight=1;
    server 192.168.8.3:80  weight=6;
    }


   server {
    #侦听80端口
        listen       80;
        #定义使用www.xx.com访问
        server_name  www.xx.com;

        #设定本虚拟主机的访问日志
        access_log  logs/www.xx.com.access.log  main;

    #默认请求
    location / {
          root   /root;      #定义服务器的默认网站根目录位置
          index index.php index.html index.htm;   #定义首页索引文件的名称

          fastcgi_pass  www.xx.com;
         fastcgi_param  SCRIPT_FILENAME  $document_root/$fastcgi_script_name;
          include /etc/nginx/fastcgi_params;
        }

    # 定义错误提示页面
    error_page   500 502 503 504 /50x.html; 
        location = /50x.html {
        root   /root;
    }

    #静态文件，nginx自己处理
    location ~ ^/(images|javascript|js|css|flash|media|static)/ {
        root /var/www/virtual/htdocs;
        #过期30天，静态文件不怎么更新，过期可以设大一点，如果频繁更新，则可以设置得小一点。
        expires 30d;
    }
    #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
    location ~ \.php$ {
        root /root;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /home/www/www$fastcgi_script_name;
        include fastcgi_params;
    }
    #设定查看Nginx状态的地址
    location /NginxStatus {
        stub_status            on;
        access_log              on;
        auth_basic              "NginxStatus";
        auth_basic_user_file  conf/htpasswd;
    }
    #禁止访问 .htxxx 文件
    location ~ /\.ht {
        deny all;
    }
    
     }
}
```

#### 2.8.2 负载均衡

```
#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
     #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #省略上文有的一些配置节点

    #。。。。。。。。。。

    #设定负载均衡的服务器列表
     upstream mysvr {
    #weigth参数表示权值，权值越高被分配到的几率越大
    server 192.168.8.1x:3128 weight=5;#本机上的Squid开启3128端口
    server 192.168.8.2x:80  weight=1;
    server 192.168.8.3x:80  weight=6;
    }

   upstream mysvr2 {
    #weigth参数表示权值，权值越高被分配到的几率越大

    server 192.168.8.x:80  weight=1;
    server 192.168.8.x:80  weight=6;
    }

   #第一个虚拟服务器
   server {
    #侦听192.168.8.x的80端口
        listen       80;
        server_name  192.168.8.x;

      #对aspx后缀的进行负载均衡请求
    location ~ .*\.aspx$ {

         root   /root;      #定义服务器的默认网站根目录位置
          index index.php index.html index.htm;   #定义首页索引文件的名称

          proxy_pass  http://mysvr ;#请求转向mysvr 定义的服务器列表

          #以下是一些反向代理的配置可删除.

          proxy_redirect off;

          #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          client_max_body_size 10m;    #允许客户端请求的最大单文件字节数
          client_body_buffer_size 128k;  #缓冲区代理缓冲用户端请求的最大字节数，
          proxy_connect_timeout 90;  #nginx跟后端服务器连接超时时间(代理连接超时)
          proxy_send_timeout 90;        #后端服务器数据回传时间(代理发送超时)
          proxy_read_timeout 90;         #连接成功后，后端服务器响应时间(代理接收超时)
          proxy_buffer_size 4k;             #设置代理服务器（nginx）保存用户头信息的缓冲区大小
          proxy_buffers 4 32k;               #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
          proxy_busy_buffers_size 64k;    #高负荷下缓冲大小（proxy_buffers*2）
          proxy_temp_file_write_size 64k;  #设定缓存文件夹大小，大于这个值，将从upstream服务器传

       }

     }
}
```

