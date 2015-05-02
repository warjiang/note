# Linux 包管理工具
### linux一般分为两大类

1. RedHat系列
    * RedHat
    * CentOS
    * Fedora
2. Debian系列
    * Debian
    * Ubuntu

### RedHat与Debian系列包管对比

  类型  | 安装包格式 | 安装命令  | 包管理工具|支持tar包？|
--------| ---------- | ----------| ----------|---------- | 
RedHat  |    rpm包   | rpm -参数 |     yum   |    Y      |
Debian  |    deb包   | dpkg -参数|   apt-get |    Y      |

### 三种安装方式
1. rpm安装
2. dkpg安装
3. tar包安装

#### rpm安装
rpm 相当于windows中的安装文件,它会自动处理软件包之间的依赖关.
1. 缺点在于,rpm一般都是预先编译好的文件,它可能已经绑定到某种CPU或者发行版上面了.
2. 优点在于,对源码具有保密性
3. 安装方式
    * 安装：rpm -ivh *.rpm　
    * 卸载：rpm -e packagename
    * 查看是否已经安装:rpm -q packagename
    * 升级：rpm -Uvh packagenaem
#### dkpg安装
#### tar包安装
tar 只是一种压缩文件格式，所以，它只是把文件压缩打包而已。
1. 优点在于,tar一般包括编译脚本，你可以在你的环境下编译，所以具有通用性。
2. 缺点在于,代码不保密
3. 安装方式:先解压缩，进入对应目录，./configure, make, make install来安装软件
4. 11111
