---
title: npm更换源
tags: node,npm
---
安装完`node`,会用`npm`安装一些node package,国内的话,用淘宝的源速度较快。之前用的是通过`npm`更换registray的方式来用淘宝的源.现在发现一款较好的工具--`nrm`[segmentdefault介绍](http://segmentfault.com/a/1190000000473869)
[淘宝npm](http://npm.taobao.org/)


### nrm 是一个 NPM 源管理器，允许你快速地在如下 NPM 源间切换：
* npm
* cnpm
* strongloop
* european
* australia
* nodejitsu
* taobao

### 安装
`npm install -g nrm`

### 使用
```markdown
nrm ls                                                      

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - http://registry.npm.taobao.org/
  eu ----- http://registry.npmjs.eu/
  au ----- http://registry.npmjs.org.au/
  sl ----- http://npm.strongloop.com/
  nj ----- https://registry.nodejitsu.com/
```
带 `*`的是当前使用的源，上面的输出表明当前源是官方源。

### 切换
`nrm use taobao`
`Registry has been set to: http://registry.npm.taobao.org/`

### 测试速度
1. `nrm test npm` (测试当前源)  
2. 测试所有源的响应时间：`nrm test`

