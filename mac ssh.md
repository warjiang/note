---
title: mac使用 
tags: secureCRT,secureFX
---
在windows下,有强大xManager,帮我们解决掉了SSH以SFTP的问题,但是Mac下的话,基本上只有靠命令行来解决了。
secureCRT:基本上等同于xManager的xShell
secureFX:基本上等同xManager的xFtp
1. 下载安装:[网盘链接](http://pan.baidu.com/s/1jGqw6nO )密码: gex7,不需要下载压缩包!
2. 破解,这里注意下,mac下基本上都是所谓的windows的绿色软件,这里我所谓的安装是,你把**软件拖到Application文件夹下!!**
3. 软件破解,secure系列的话,跟其他软件不同,secureCRT和secureFX需要分开破解
4. secureCRT破解:(依赖`securecrt_mac_crack.pl`的`perl`脚本 )
```markdown
perl /Users/apple/Desktop/chrome/securecrt_mac_crack.pl  /Applications/SecureCRT.app/Contents/MacOS/SecureCRT
```
这里说明下 `/Users/apple/Desktop/chrome/securecrt_mac_crack.pl`是我的`securecrt_mac_crack.pl`的路径
`/Applications/SecureCRT.app/Contents/MacOS/SecureCRT`是我`secureCRT`存放的路径,上面特别提醒了要把`secureCRT`拖到`Application`里面,如果拖了的话,那么这里的`secureCRT`路径就跟我一样。
perl脚本执行完毕,会生成注册码,这里又要提醒下,安装`secureCRT`需要断网。
```
crack successful

License:

	Name:		bleedfly
	Company:	bleedfly.com
	Serial Number:	03-29-002542
	License Key:	ADGB7V 9SHE34 Y2BST3 K78ZKF ADUPW4 K819ZW 4HVJCE P1NYRC
	Issue Date:	09-17-2013
```
然后点击continue,`secureCRT`提示错误,这个时候,我们选择手动输入即可。
5. secureFX破解:
secureFX的破解跟secureCRT雷同,只不过secureFX如果第一次没有破解成功,需要关闭重来。
```
crack successful

License:

	Name:		xiaobo_l
	Company:	www.boll.me
	Serial Number:	06-70-000873
	License Key:	ACUYJV Q1V2QU 1YWRCN NBYCYK ADTANN 94K1WD WECKN4 Y3F249
	Issue Date:	09-30-2013
```

### 解决secureCRT不保存密码问题[引用自百度经验](http://jingyan.baidu.com/article/915fc414fda5fb51394b20bd.html)
这里注意下,我给的安装包是7.3.1的,需要用后一种情况。