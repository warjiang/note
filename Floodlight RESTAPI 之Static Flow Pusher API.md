---
title:  Floodlight RESTAPI 之Static Flow Pusher API
tags: Floodligth,RESTAPI,Static Flow Pusher API
---
## 什么是Static Flow Pusher API
`Static Flow Pusher`是`Floodlight`的一个模块,对外通过`REST API`的接口暴露给用户使用,允许用户手动在`OpenFlow`网络中插入流
## 预插入流与被动插入流
`OpenFlow`支持两种插入流的模式:**预插入**和**被动插入**.被动插入法流发生在当一个数据包到达OpenFlow交换机,但是没有找到匹配流的的情况.这个包就被发送到控制器,控制器对这个数据包完成路径计算,增加适当的流,并让这个数据包继续被发送.或者,流表可以在数据包到达交换机之前,通过控制器插入到交换机内.在预先插入流的情况下,即将到来的数据包将不会被发送到控制器做路径规划,因为已经在交换机中能匹配预先插入的流了.
`Floodlight`支持这两种插入的流的模式.`Static Flow Pusher`在预先插入流的情况下更有用.
**注意:默认情况下,Floodlight加载了`Forwarding module`,在你push static flow的时候，也会做被动流的push,如果想要单独使用静态流,必须从`floodlight.properties`内移除转发模块**