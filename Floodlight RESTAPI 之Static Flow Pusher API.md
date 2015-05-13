---
title:  Floodlight RESTAPI 之Static Flow Pusher API
tags: Floodligth,RESTAPI,Static Flow Pusher API
---
## 什么是Static Flow Pusher API
`Static Flow Pusher`是`Floodlight`的一个模块,对外通过`REST API`的接口暴露给用户使用,允许用户手动在`OpenFlow`网络中插入流
## 预插入流与被动插入流
`OpenFlow`支持两种插入流的模式:**预插入**和**被动插入**.被动插入法流发生在当一个数据包到达OpenFlow交换机,但是没有找到匹配流的的情况.这个包就被发送到控制器,控制器对这个数据包完成路径计算,增加适当的流,并让这个数据包继续被发送.或者,流表可以在数据包到达交换机之前,通过控制器插入到交换机内.在预先插入流的情况下,即将到来的数据包将不会被发送到控制器做路径规划,因为已经在交换机中能匹配预先插入的流了.
`Floodlight`支持这两种插入的流的模式.`Static Flow Pusher`在预先插入流的情况下更有用.
**注意:默认情况下,Floodlight加载了`Forwarding module`,在你push static flow的时候,也会做被动流的push,如果想要单独使用静态流,必须从floodlight.properties内移除转发模块**
## 怎样使用Static Flow Pusher
### API概览
<style>
    table{
        border-collapse: collapse;
        border: none;
    }
    td{
        border: solid #000 1px;
    }
</style>
<table>
    <tr>
        <td>URI</td>
        <td>Description</td>
        <td>Arguments</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>/wm/staticflowpusher/list/<switch>/json</td>
        <td>List static flows for a switch or all switches</td>
        <td>switch: Valid Switch DPID (XX:XX:XX:XX:XX:XX:XX:XX) or "all"</td>
        <td>列出一个或多个交换机的静态流(switch DPID,Floodlight组织的SDN网络中交换机的唯一标识符)</td>
    </tr>
    <tr>
        <td>/wm/staticflowpusher/json</td>
        <td>Add/Delete static flow</td>
        <td>HTTP POST data (add flow), HTTP DELETE (for deletion)</td>
        <td>POST增加流,DELETE删除流</td>
    </tr>
    <tr>
        <td>/wm/staticflowpusher/clear/<switch>/json</td>
        <td>Clear static flows for a switch or all switches</td>
        <td>switch: Valid Switch DPID (XX:XX:XX:XX:XX:XX:XX:XX) or "all"
</td>
        <td>清楚一个或者所有交换机的静态流</td>
    </tr>
</table>


### 增加一个流
Static Flow Pusher可以通过REST API访问到。想要增加一个静态流,需要按照JSON的格式定义一条静态流.比如,想要在交换机1上这样一个流,可以获取所有从port1进来的数据包,并让他们从port2出去.只需要组织JSON字符串并用简单的curl命令来把HTTP请求POST到控制器上就可以了.你可以用上面的第二条命令来dump处这条流.
```JSON
curl -d 
    '{
        "switch": "00:00:00:00:00:00:00:01",
        "name":"flow-mod-1", 
        "cookie":"0",
        "priority":"32768", 
        "in_port":"1",
        "active":"true", 
        "actions":"output=2"
    }'
http://<controller_ip>:8080/wm/staticflowpusher/json
```

### 列出静态流
为了获取所有的静态流,Static Flow Pusher的REST API在HTTP GET请求中接受上面API Summary中指定的path参数。你可以做基于单交换机的查询静态流或者向控制器请求所有交换机的所有的静态流.
```
curl 
http://<controller_ip>:8080/wm/staticflowpusher/list/00:00:00:00:00:00:00:01/json
curl 
http://<controller_ip>:8080/wm/staticflowpusher/list/all/json
```

### 删除一条静态流
发送一条包含静态流名字的HTTP DELETE请求,可以删除一条静态流.静态流的名字在插入的时候指定.
```
curl -X DELETE -d 
    '{
        "name":"flow-mod-1"
     }'
http://<controller_ip>:8080/wm/staticflowpusher/json
```


## 静态流条目可以大概分为x部分:

1. Required properties of a flow entry(流条目的必选属性)
2. Optional properties of a flow entry(流条目的可选属性)
3. Optional match fields of a flow entry(流条目的可选匹配字段)
4. Possible Action and Instruction Lists(可能的行为和指令)
5. Possible Actions within the Action and Instruction fields()


### 静态流条目必选属性
<table>
<tr>
<td>key</td>
<td>value</td>
<td>notes</td>
</tr>
<tr>
<td>name</td>
<td>string</td>
<td>流条目的名字.name字段作为静态流的主键,name字段必须是全局唯一的
</td>
</tr>
<tr>
<td>switch</td>
<td>switch DPID</td>
<td>DPID of the switch to which this flow should be added. 
xx:xx:xx:xx:xx:xx:xx:xx</td>
</tr>
<tr></tr>
<tr></tr>
</table>
### 静态流条目的可选参数
<table>
<tr>
<td>Key</td>
<td>Value</td>
<td>Notes</td>
</tr>
<tr>
<td>priority</td>
<td>number</td>
<td>Default is 32767. Max is 32767</td>
</tr>
<tr>
<td>active</td>
<td>boolean</td>
<td>"true" or "false"</td>
</tr>
<tr>
<td>table</td>
<td>number</td>
<td> 如果这个参数忽略,默认流表将被使用</td>
</tr>
<tr>
<td>idle_timeout</td>
<td>number</td>
<td>Default of zero, which is no-timeout.</td>
</tr>
<tr>
<td>hard_timeout</td>
<td>number</td>
<td>Default of zero, which is no-timeout.</td>
</tr>
<tr>
<td>cookie</td>
<td>number</td>
<td>Can be hexadecimal (with leading 0x) or decimal.</td>
</tr>
<tr>
<td>cookie_mask</td>
<td>number</td>
<td>Can be hexadecimal (with leading 0x) or decimal.</td>
</tr>
</table>






