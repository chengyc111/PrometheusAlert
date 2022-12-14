# PrometheusAlert全家桶钉钉配置说明

-----------------

 **开启钉钉机器人**

打开钉钉,进入钉钉群中,选择群设置-->智能群助手-->添加机器人-->自定义，可参下图：

![ding](../images/dingding1.png)

![ding2](../images/dingding2.png)

新版本的钉钉加了安全设置,只需选择安全设置中的 自定义关键词 即可,并将关键词设置为 Prometheus或者app.conf中设置的title值均可,参考下图

![ding3](../images/dingding3.png)

![ding4](../images/dingding4.png)

复制图中的Webhook地址，并填入PrometheusAlert配置文件app.conf中对应配置项即可。

 **PS: 钉钉机器人目前已经支持 `@某人` ,使用该功能需要取得对应用户的钉钉关联手机号码，如下图：**
![ding4](../images/dingding5.png)

钉钉目前支持只支持markdown语法的子集，具体支持的元素如下：

```
标题
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

引用
> A man who stands for nothing will fall for anything.

文字加粗、斜体
**bold**
*italic*

链接
[this is a link](http://name.com)

图片
![](http://name.com/pic.jpg)

无序列表
- item1
- item2

有序列表
1. item1
2. item2
```

钉钉相关配置：

```
#---------------------↓全局配置-----------------------
#告警消息标题
title=PrometheusAlert
#钉钉告警 告警logo图标地址
logourl=https://raw.githubusercontent.com/feiyu563/PrometheusAlert/master/doc/alert-center.png
#钉钉告警 恢复logo图标地址
rlogourl=https://raw.githubusercontent.com/feiyu563/PrometheusAlert/master/doc/alert-center.png

#---------------------↓webhook-----------------------
#是否开启钉钉告警通道,可同时开始多个通道0为关闭,1为开启
open-dingding=1
#默认钉钉机器人地址
ddurl=https://oapi.dingtalk.com/robot/send?access_token=xxxxx
#是否开启 @所有人(0为关闭,1为开启)
dd_isatall=1
```

**如何使用**

以Prometheus配合自定义模板为例：

Prometheus配置参考：

```
global:
  resolve_timeout: 5m
route:
  group_by: ['instance']
  group_wait: 10m
  group_interval: 10s
  repeat_interval: 10m
  receiver: 'web.hook.prometheusalert'
receivers:
- name: 'web.hook.prometheusalert'
  webhook_configs:
  - url: 'http://[prometheusalert_url]:8080/prometheusalert?type=dd&tpl=prometheus-dd&ddurl=钉钉机器人地址,钉钉机器人地址2&at=18888888888,18888888889'
```