# http协议
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的 WWW（万维网） 文件都必须遵守这个标准。设计 HTTP 最初的目的是为了提供一种发布和接收 HTML 页面的方法。（百度百科）

## http状态码详解
#### 50x 状态码的具体含义如下：

- 500 Internal Server Error：服务器遇到了内部错误，无法处理请求。
    这可能是由于服务器软件错误、硬件故障或其他原因造成的。
- 501 Not Implemented：服务器不支持请求的方法。
    这可能是由于服务器不支持请求的方法或请求的方法不正确等原因造成的。
- 502 Bad Gateway：服务器作为网关或代理，从上游服务器获取资源时发生了错误。
    这可能是由于上游服务器错误、网络故障或其他原因造成的。

- 503 Service Unavailable：服务器暂时无法处理请求。
    这可能是由于服务器维护、过载或其他原因造成的。
- 504 Gateway Timeout：服务器作为网关或代理，从上游服务器获取资源时超时。
    这可能是由于上游服务器响应超时、网络故障或其他原因造成的。

- 505 HTTP Version Not Supported：服务器不支持请求的 HTTP 版本。
    这可能是由于请求的 HTTP 版本不正确或服务器不支持请求的 HTTP 版本等原因造成的。

- 510 Not Extended：服务器不支持请求的扩展。
    这可能是由于请求的扩展不正确或服务器不支持请求的扩展等原因造成的。


#### 40x 状态码的具体含义如下：

- 400 Bad Request：请求无效，服务器无法理解。
    这可能是由于请求格式不正确、请求参数不正确或请求方法不正确等原因造成的。
- 401 Unauthorized：请求需要身份验证。
    这可能是由于请求没有携带有效的身份验证凭据或请求的资源要求身份验证等原因造成的。

- 403 Forbidden：请求被拒绝。
    这可能是由于请求的资源不允许访问或请求的用户没有权限访问该资源等原因造成的。
- 404 Not Found：请求的资源不存在。
    这可能是由于资源已被删除或资源的 URL 不正确等原因造成的。

- 405 Method Not Allowed：请求方法不允许。
    这可能是由于请求的资源不允许使用该方法或请求的方法不支持等原因造成的。

- 406 Not Acceptable：请求的格式不支持。
    这可能是由于服务器不支持请求的格式或请求的格式与资源的格式不匹配等原因造成的。

- 407 Proxy Authentication Required：请求需要通过代理进行身份验证。
    这可能是由于请求的资源要求通过代理进行身份验证或请求的用户没有权限访问该资源等原因造成的。

- 408 Request Timeout：请求超时。
    这可能是由于客户端没有及时发送请求或服务器没有及时响应请求等原因造成的。

- 409 Conflict：请求与现有资源发生冲突。
    这可能是由于请求的资源已被修改或请求的资源与现有资源冲突等原因造成的。

- 410 Gone：请求的资源已永久删除。
    这可能是由于资源已被永久删除或资源的 URL 已更改等原因造成的。

- 411 Length Required：请求必须包含 Content-Length 头部。
    这可能是由于请求的方法要求包含 Content-Length 头部或请求的资源要求包含 Content-Length 头部等原因造成的。

- 412 Precondition Failed：请求的先决条件不满足。
    这可能是由于请求的条件不满足或请求的条件与服务器的状态不匹配等原因造成的。

- 413 Request Entity Too Large：请求实体太大。
    这可能是由于请求的资源不允许请求实体太大或请求的实体太大而无法处理等原因造成的。

- 414 Request-URI Too Long：请求 URI 太长。
    这可能是由于请求的资源不允许请求 URI 太长或请求的 URI 太长而无法处理等原因造成的。

- 415 Unsupported Media Type：请求的媒体类型不支持。
    这可能是由于服务器不支持请求的媒体类型或请求的媒体类型与资源的媒体类型不匹配等原因造成的。

- 416 Requested Range Not Satisfiable：请求的范围不可用。
    这可能是由于请求的资源不允许请求范围或请求的范围不符合服务器的条件等原因造成的。

- 417 Expectation Failed：请求的期望不满足。

#### 30x 状态码的具体含义如下：

- 301 Moved Permanently：资源已永久移动到另一个 URI。
- 302 Found：资源已临时移动到另一个 URI。
- 303 See Other：客户端应该使用 GET 方法访问另一个 URI。
- 304 Not Modified：资源未修改，客户端可以使用缓存的版本。
- 305 Use Proxy：客户端应该使用代理来访问另一个 URI。
- 307 Temporary Redirect：资源已临时移动到另一个 URI，客户端应该使用相同的方法访问新 URI。
- 308 Permanent Redirect：资源已永久移动到另一个 URI，客户端应该使用相同的方法访问新 URI。

#### 20x 状态码的具体含义：

- 200 OK：请求成功。
- 201 Created：请求已成功创建新资源。
- 202 Accepted：请求已被接受，但处理尚未完成。
- 203 Non-Authoritative Information：请求成功，但响应中的信息可能不是最新的。
- 204 No Content：请求成功，但响应体为空。
- 205 Reset Content：请求成功，要求客户端重置表单。
- 206 Partial Content：请求成功，但只有部分资源已被传输。

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
