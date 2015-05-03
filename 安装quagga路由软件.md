# 安装quagga路由软件
## CentOS:
CentOS下安装quagga很容易，直接使用yum install quagga -y就可以了
## Ubuntu:
Ubuntu安装quagga
1. 利用apt-get install quagga
2. 下载源码，手动安装quagga


Ubuntu手动安装quagga:
1. 机器环境:Ubuntu 15.04, quagga 0.99.20([quagga下载地址](http://download.savannah.gnu.org/releases/quagga/quagga-0.99.20.tar.gz))
2. wget下载,tar解压
3. cd quagga目录,
配置./configure
```markdown
./configure 
--enable-vtysh 
--enable-user=root 
--enable-group=root
--enable-vty-group=root
```
执行完毕会打印出配置信息
```markdown
Quagga configuration
--------------------
quagga version          : 0.99.20
host operating system  : linux-gnu
source code location    : .
compiler                : gcc
compiler flags          : -Os -fno-omit-frame-pointer -g -std=gnu99 -Wall -Wsign-compare -Wpointer-arith -Wbad-function-cast -Wwrite-strings -Wmissing-prototypes -Wmissing-declarations -Wchar-subscripts -Wcast-qual
make                    : make
includes                :
linker flags            :  -lcrypt   -lrt   -ltermcap -lreadline -lm
state file directory    : /var/run
config file directory   : /usr/local/etc
example directory       : /usr/local/etc
user to run as          : root
group to run as         : root
group for vty sockets   : root
config file mask        : 0600

```
4. make & make install

```markdown
安装过程中可能会遇到的问题:
执行./configure出现configure: error: GNU awk is required for lib/memtype.h made by memtypes.awk.BSD awk complains: awk: gensub doesn't support backreferences (subst "\1") 
```
__解决方法__ : ```apt-get install gawk```

```
configure: error: vtysh needs libreadline but was not found and usable on your system.

```
__解决方法__ : ```apt-get install libreadline6-dev```


```markdown
安装完成,执行zebra -d
zebra: error while loading shared libraries: libzebra.so.0: cannot open shared object file: No such file or directory
```
__解决方法__:
```
cp lib*.* /lib 
rm lib*.*
```

安装完成之后:配置文件存放在/usr/local/etc,如果要启动zebra,则把quagga官方提供的example.conf拷贝过来，重命名为zebra.conf,同时可以简单的配置route name,password.



```markdown
执行zebra -d启动zebra
执行netstat -ntulp|grep :2601如果有匹配条目,则说明quagga安装成功.
可以使用telnet localhost 2601登录zebra控制台
```


