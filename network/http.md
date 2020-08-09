# http协议
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的 WWW（万维网） 文件都必须遵守这个标准。设计 HTTP 最初的目的是为了提供一种发布和接收 HTML 页面的方法。（百度百科）

## http状态码详解
500: 服务异常
501: 服务器还是不具有请求功能的
502: 服务器上的一个错误网关
503: 服务不可用的一种状态
504: 网管超时

404: 未找到相应服务
403:
401:

301: 永久性
302: 暂时性

200: ok
201:
202:

## get请求和post请求区别

1、GET请求一般用去请求获取数据，POST一般作为发送数据到后台时使用

2、GET请求也可传参到后台，但是其参数在浏览器的地址栏的url中可见，所以隐私性安全性较差，且参数长度也是有限制的，POST请求传递参数放在Request body中，不会在url中显示，比GET要安全，且参数长度无限制

3、GET 请求可被缓存，POST 请求不会被缓存

4、GET请求只能进行url编码（application/x-www-form-urlencoded），POST支持多种编码方式（application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。）


## http和https的区别？

## websocket如何握手升级？

## https认证过程

## http2协议

## http2和http1的区别
