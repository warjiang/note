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
    * 安装:rpm -ivh *.rpm　
    * 卸载:rpm -e packagename
    * 查看是否已经安装:rpm -q packagename
    * 升级:rpm -Uvh packagename
    * 查询所有安装的包:rpm -qa
    * 查询某个包:rpm -qa | grep packagename/rpm -qi packagename
    * 查询软件的安装路径:rpm -ql packagename/rpm -qc packagename
    * 查询某个文件是那个rpm包产生:rpm -qf /etc/yum.conf
#### dkpg安装
1. 安装方式
    * dpkg -i <package.deb> 安装一个 Debian 软件包，如你手动下载的文件。
    * dpkg -c <package.deb> 列出 <package.deb> 的内容.
    * dpkg -I <package.deb> 从 <package.deb> 中提取包裹信息。
    * dpkg -r <package> 完全清除一个已安装的包裹,remove 只是删掉数据和可执行文件
    * dpkg -P <package> 完全清除一个已安装的包裹,purge另外还删除所有的配制文件。
    * dpkg -L <package> 列出 <package> 安装的所有文件清单。
    * dpkg -s <package> 显示已安装包裹的信息
    * dpkg-reconfigure <package> 重新配制一个已经安装的包裹，如果它使用的是 debconf (debconf 为包裹安装提供了一个统一的配制界面)。
#### tar包安装
tar 只是一种压缩文件格式，所以，它只是把文件压缩打包而已。
1. 优点在于,tar一般包括编译脚本，你可以在你的环境下编译，所以具有通用性。
2. 缺点在于,代码不保密
3. 安装方式:先解压缩，进入对应目录，./configure, make, make install来安装软件

#### 使用yum和apt-get
直接安装包就是以上三种方式，但是并不考虑各个包之间的依赖关系.yum和apt-get适合做包管理工具

1. RedHat包管理工具 yum
    * 安装软件包:yum install <package_name>
    * 删除软件包:yum remove <package_name>
    * 查询软件信息:我们常会碰到这样的情况,想要安装一个软件,只知道它和某方面有关,但又不能确切知道它的名字.这时yum的查询功能就起作用了,你可以用yum search keyword这样的命令来进行搜索,比如我们要则安装一个Instant Messenger,但又不知到底有哪些，这时不妨用 yum search messenger这样的指令进行搜索,yum会搜索所有可用rpm的描述,列出所有描述中和messeger有关的rpm包,于是我们可能得到gaim，kopete等等,并从中选择.有时我们还会碰到安装了一个包，但又不知道其用途，我们可以用yum info packagename这个指令来获取信息
        1. 查找软件包:yum search <keyword>
        2. 列出所有可安装的软件包:yum list
        3. 列出所有可更新的软件包:yum list updates
        4. 列出所有已安装的软件包:yum list installed
        5. 列出所有已安装但不在 Yum Repository 內的软件包:yum list extras
        6. 列出所指定的软件包:yum list <package_name>
2. Debian包管理工具 apt-get
使用apt-get除了便捷以外,apt-get的一大好处是极大地减小了所谓依赖关系恶梦的发生几率(dependency hell),即使是陷入了dependency hell,apt-get也提供了很好的援助手段，帮你逃出魔窟。
通常 apt-get 都和网上的压缩包一起出没，从互联网上下载或是安装。全世界有超过200个 debian 官方镜像,还有繁多的非官方软件包提供网站.你所使用的基于Debian的发布版不同,你所使用的软件仓库可能需要手工选择或是可以自动设置.你能从Debian官方网站得到完整的镜像列表.而很多非官方网站提供各种特殊用途的非官方软件包,当然,使用非官方软件包会有更多风险了.
    * 搜索包:apt-cache search package
    * 获取包的相关信息,如说明、大小、版本等:apt-cache show package
    * 安装包: apt-get install package
    * 重新安装包:apt-get install package - - reinstall
    * 修复安装("-f = --fix-missing"):apt-get -f install 
    * 删除包:apt-get remove package
    * 删除包,包括删除配置文件等:apt-get remove package --purge 
    * 更新源:apt-get update 
    * 更新已安装的包:apt-get upgrade
    * 升级系统: apt-get dist-upgrade
    * 使用dselect 升级:apt-get dselect-upgrade
    * 了解使用依赖:apt-cache depends package 
    * 查看该包被哪些包依赖:apt-cache rdepends package 
    * 安装相关的编译环境: apt-get build-dep package
    * 下载该包的源代码:apt-get source package
    * 清理无用的包:apt-get clean && apt-get autoclean
    * 检查是否有损坏的依赖:apt-get check