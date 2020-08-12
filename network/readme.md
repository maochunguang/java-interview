#  网络相关知识，http，tcp等等。
## pc一次请求的流程
1. 根据域名找服务器ip，首先根据host判断，如果host存在，直接使用该ip。
2. 如果host不存在，走dns服务器，电脑配置的dns，如果没有配置走默认的dns，一般是运营商的dns服务器。
3. 通过dns找到ip，访问ip，进行tcp握手等操作。

> 参考《tcp-ip详解》，《http权威指南》
