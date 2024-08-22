##### 什么是 HTTP

> HTTP(HyperText Transfer Protocal) 超文本传输协议，是一种客户端-服务器协议，处于应用层，当访问网页的时候，浏览器和 Web 服务器之间就会通过 HTTP 在 Internet 上进行数据的发送和接收。

![HTTP](/Users/liangmenglin/Library/Application Support/typora-user-images/image-20240821145019780.png)

> 客户端通过发起请求，服务器响应请求的方式进行通信，客户端通常 Web 浏览器。

![image-20240821145214328](/Users/liangmenglin/Library/Application Support/typora-user-images/image-20240821145214328.png)

##### HTTP 的特点

- **可靠传输**: HTTP 基于 TCP/IP
- **请求-响应**
- **无状态**：每次 HTTP 请求都是独立的，无关的，默认不需要保留通信过程的上下文信息
- **可扩展**：通过客户端和服务器之间的新标头的语义达成简单的协议来引入新功能

##### HTTP 的资源标志

> 统一资源标志符 URI。用于标志互联网资源名称的字符串。URI 可以进一步分为 URL 和 URN，URI 是一种抽象的，高层次概念定义统一资源标志，而 URL 和 URN 则是具体的资源标志方式。

**URI 的结构**

```javascript
[scheme][://]user:passwd@[host:port][path][?query][#fragment]
```

- **scheme**: 协议名，必须和 :// 链接在一起
- **user:password**: 登录用户的信息，不安全，不常用
- **host:port**: 主机名和端口
- **path**: 请求路径
- **query**： 查询参数，一般为 key=value 这种形式，多个键值之间通过&分开
- **Fragment**：表示 URI 所定位的资源内一个瞄点，浏览器可以根据这个瞄点跳转到对应的位置

**编码方式**

> URI 只能通过 ASCII，非 ASCII 的自负和界定符转为十六进制字节值，然后在前面加个%.如：<span style="color: red;font-weight: bolder">空格转为%20</span>

###### **URL 统一资源定位符**

> 是 URI 最常见的形式

###### **URN 永久性统一资源定位符**

> 另外一种形式的 URI，通过特定命名空间中的唯一名称来标志资源

```
urn:isbn:9780141036144
urn:ietf:rfc:7230
```

###### **Data URL Scheme**

> 前缀为 data: 协议的 URL，允许内容创建者向文档中嵌入小文件

由四个部分组成

```
data:[<mediatype>][;charset=<charset>][;<encoding>],<encoded-data>
```

- 协议头 data:

- MIME 类型: 媒体类型，是一种标准，用于表示文档，文件，字节流的性质和格式

  | 类型        | 描述           | 示例                                                                                                                                                                   |
  | ----------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | text        | 普通文件，可读 | <span style="color: #f2a441;font-weight: bolder">text/plain</span>                                                                                                     |
  | image       | 图像文件       | <span style="color: #f2a441;font-weight: bolder">image/gif , image/jpeg, image/bmp, image/webp, image/x-icon, image/vnd.microsoft.icon,image/sgv+xml, image/png</span> |
  | Audio       | 音频文件       | <span style="color: #f2a441;font-weight: bolder">audio/midi, audio.mpeg, audio/webm,audio/ogg,audio/wav</span>                                                         |
  | Video       | 视频文件       | <span style="color: #f2a441;font-weight: bolder">video/webm,video/ogg</span>                                                                                           |
  | Application | 二进制数据     | <span style="color: #f2a441;font-weight: bolder">Application/octet-stream,application/pkcs12, application/vnd.mspowerpoint</span>                                      |

- 源文本的字符集编码方式：[;charset=<charset>],默认的编码是 charset=US-ASCII,即数据部分的每个字符都会自动编码为%xx

- 数据编码方式[;<encoding>]: 默认 US-ASCII 和 BASE64 两种

- 编码后的数据<encoded data>

```
<img src="data:image/x-icon;base64,AAABAAEAEBAAAAAAAABoBQAAF..." />
<link type="text/css" href="data:text/css;base64,LyogKioqKiogVGVtcGxhdGUgKioq" />
<link type="text/javascript" href="data:text/javascript;base64,LyogKioqKiogVGVtcGxhdGUgKioq" />
img {
  width: 100px;
  height: 100px;
  background-image: url(data:image/x-icon;base64,AAABAAEAEBAAAAAAAABoBQAAF...);
}
```

#####　 HTTP 的报文格式

> http 报文是面向文本的，报文中的每个字段都是 ASCII。HTTP 有两类报文：请求报文和响应报文

###### HTTP 请求报文组成

- 请求行
- HTTP 头部字段
- 空行
- (可选)HTTP 报文主体数据

![image-20240821161602823](/Users/liangmenglin/Library/Application Support/typora-user-images/image-20240821161602823.png)

**请求行**

- 请求方法，GET，POST,PUT,DELETE 等

- 请求地址 URL

- HTTP 协议版本：如 HTTP/1.1

  ```
  GET /index.html HTTP/1.1
  ```

| 方法名  | 功能                                                                                                                                          |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| GET     | 显示请求，只能用在读取数据，入参拼接在 URL 上，多次请求相同结果                                                                               |
| POST    | 向服务器提交数据或发送数据来创建资源。数据包含在请求的实体部分中而不是 URL，多次请求可能产生不同的结果                                        |
| PUT     | 向服务器上传文件或资源，通常是创建或替换某个资源。数据包含在请求的实体，多次请求相同结果                                                      |
| DELETE  | 请求服务器删除 Request-URL 所标识得资源                                                                                                       |
| OPTIONS | 用于查询服务器支持的请求方式或跨域请求时的预检请求。返回允许的 HTTP 方法列表多次请求相同结果                                                  |
| TRACE   | 用于在请求过程当中对路径进行回显                                                                                                              |
| HEAD    | 和 GET 一样，都是先服务发出指定资源的请求，只不过服务器不回传资源的文本部分。好处在于不必传输全部内容的情况下，就可以获取其中关于该资源的信息 |
| CONNECT | HTTP/1.1 中，用户建立到服务器的隧道连接，通常用于 SSL 加密服务器的连接                                                                        |

GET/POST

> HTTP 协议并未规定 GET/POST 的请求长度限制是多少，对 GET 请求参数的限制是来源于浏览器和 WEB 服务器，限制了 URL 的长度。

**请求头部字段**

包含附加信息用于描述请求的客户端，请求的内容，期望的响应

```
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html,application/xhtml+xml
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Connection: keep-alive
Content-Length: 21429
Content-Type: application/json
Host: api.github.com
Origin: https://github.com
Referer: https://github.com/
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36
```

常见的请求头

| 请求头            | 说明                               |
| ----------------- | ---------------------------------- |
| Accept            | 浏览器接受的数据类型               |
| Accept-Encoding   | 浏览器接受的数据压缩格式           |
| Host              | 表示当前请求访问的目标地址         |
| Authorization     | 表示用户身份证信息                 |
| User-Agent        | 客户端浏览器信息                   |
| If-Modified-Since | 表示去星球资源最近一次更新时间     |
| If-None-Match     | 当前请求资源最近一次标志的 ETag 值 |
| Cookie            | 保存的 Cookie                      |
| Referer           | 标识请求引用自哪个地址             |

**空行**

用于分隔头部和实体，表示头部的结束，即使没有请求体，空行也是必须的

**请求实体**

请求实体通常用于 POST，PUT 等方法，包含了要发送到服务器的数据

```
username=user&password=pass
```

###### HTTP 响应报文

![image-20240821175023950](/Users/liangmenglin/Library/Application Support/typora-user-images/image-20240821175023950.png)

报文组成

- 状态行

  ```
  HTTP/1.1 200 OK
  ```

  - HTTP 版本

  - 状态码

    | 状态码 | 对应信息                                      |
    | ------ | --------------------------------------------- |
    | 1XX    | 提示信息，表示接受已收到，继续处理            |
    | 2XX    | 用于表示请求已被成功接收，理解                |
    | 3XX    | 表示资源被永久转移到其他 URL，重定向          |
    | 4XX    | 客户端错误请求-请求有语法错误或者请求无法实现 |
    | 5XX    | 服务器端错误-服务器未能实现合法的请求         |

  - 状态描述

- 响应头

  ```
  Content-Type: text/html 响应内容的类型
  Content-Length: 1234 响应内容的字节长度
  Server: Apache/2.4.1 服务器信息
  ```

  | 名称              | 作用                               |
  | ----------------- | ---------------------------------- |
  | Date              | 当前响应资源发送的服务器日期和时间 |
  | Last-Modified     | 当前响应资源最后被修改的服务器时间 |
  | Transfer-Encoding | 当前响应资源传输实体的编码格式     |
  | Set-Cookie        | 表示设置 Cookie 的信息             |
  | Location          | 在重定向中或者创建新资源时使用     |
  | Server            | 服务器名称                         |
  | Content-Length    | 响应体的长度                       |

- 空行

  用于分割头部和实体，表示头部的结束

- 响应体

##### HTTP Header

> Header 用于描述报文，其字段名不区分大小写，不允许出现空格，不可以出现下划线，字段名后必须紧跟：

###### **报文信息**

| 报文形式 | 字段名     | 说明                                                   | 示范                                                               |
| -------- | ---------- | ------------------------------------------------------ | ------------------------------------------------------------------ |
| 通用     | Date       | 创建报文的日期时间                                     | `Date: Tue, 15 Nov 2010 08:12:31 GMT`                              |
| 请求头   | Origin     | 请求页面的站点地址                                     | `Origin: https://developer.mozilla.org`                            |
|          | Referer    | 请求页面的完整 URL 地址                                | `Referer: https://developer.mozilla.org/en-US/docs/Web/JavaScript` |
|          | Host       | 发送的服务器的主机名和端口号                           | `Origin: https://developer.mozilla.org`                            |
|          | User-Agent | 用户代理软件的应用类型，操作系统，软件开发商以及版本号 | `User-Agent: Mozilla/5.0 (Linux; X11)`                             |

###### 网络连接

| 报文形式 | 字段名     | 说明                                                             | 示范                                                                                                                                  |
| -------- | ---------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 通用     | Keep-Alive | 允许消息发送者表示连接的状态，还可以设置超时时长和最大请求数     | `Keep-Alive: timeout=5, max=1000;timeout: 指定空闲连接需要保持打开状态的最小时长;max: 在连接关闭之前，再次连接可以发送的请求的最大值` |
|          | Connection | 决定当前事务完成后，是否关闭网络连接<br/>可取值 keep-alive,close | `keep-alive: 表示客户端想要保持该网络连接打开;close: 表示客户端或服务器想要关闭该网络连接，是默认值`                                  |

示范使用

```html
HTTP/1.1 200 OK Connection: Keep-Alive Content-Encoding: gzip Content-Type: text/html; charset=utf-8 Date: Thu, 11 Aug 2016 15:23:13 GMT Keep-Alive: timeout=5, max=1000 Last-Modified: Mon, 25 Jul 2016
04:32:39 GMT Server: Apache
```

HTTP 请求启动 keep-alive 需要服务端配合，Nginx 配置

```
http {
  # 客户端连接在服务器端保持开启的超时值
  keepalive_timeout  120s 120s;
  # 可以服务的请求的最大数量
  keepalive_requests 10000;
}
```

###### 内容协商

| 报文形式   | 字段名           | 说明                                 |                   示范                   |
| ---------- | ---------------- | ------------------------------------ | :--------------------------------------: |
| **请求头** | Accept           | 告知服务器客户端可处理的媒体类型     |     `Accept: text/plain, text/html`      |
|            | Accept-Charset   | 告知客户端服务器可以处理的字符集类型 |   `Accept-Charset: utf-8, iso-8859-5`    |
|            | Accept-Encoding  | 告知服务器客户端可处理的内容编码方式 |   `Accept-Encoding: gzip, deflate, br`   |
|            | Accept-Language  | 告知服务器客户端可处理的自然语言     |         `Accept-Language: en,zh`         |
| **响应头** | Content-Type     | 指示资源的媒体类型                   | `Content-Type: text/html; charset=utf-8` |
|            | Content-Encoding | 表示资源的编码方式                   |         `Content-Encoding: gzip`         |
|            | Content-Language | 表示资源的自然语言                   |        `Content-Language: en,zh`         |
|            | Content-Length   | 指示资源的体积大小                   |          `Content-Length: 348`           |
|            | Content-Location | 指示要访问的资源通过内容协商后的 URL |      `Content-Location: /index.htm`      |
|            | Content-Range    | 数据片段在整个文件中的位置           | `Content-Range: bytes 21010-47021/47022` |

压缩方式：gzip, deflate, br

```
<!-- 发送端 -->
Content-Encoding: gzip;
<!-- 接收端 -->
Accept-Encoding: gzip
```

Content-Type

- media-type：资源或数据的 MIME 类型
- charset：字符编码标准
- boundary：用于封装消息的多个部分的边界

###### 同源策略

| 报文形式 | 首部字段名                       | 说明                                                                                           |                               示例                               |
| :------: | -------------------------------- | ---------------------------------------------------------------------------------------------- | :--------------------------------------------------------------: |
|  请求头  | Access-Control-Request-Header    | 预检请求，列出正式请求中允许的首部信息                                                         |               `Access-Control-Request-Headers: *`                |
|          | Access-Control-Reques-Method     | 预检请求，列出正式请求中允许的请求方法                                                         |                `Access-Control-Request-Method: *`                |
|  响应头  | Access-Control-Allow-Credentials | 表示是否可以将对请求的响应暴露给页面                                                           |             `Access-Control-Allow-Credentials: true`             |
|          | Access-Control-Allow-Headers     | 预检请求，列出正式请求中允许的首部信息                                                         |                `Access-Control-Allow-Headers: *`                 |
|          | Access-Control-Allow-Methods     | 预检请求，列出正式请求中允许的请求方法                                                         |                `Access-Control-Allow-Methods: *`                 |
|          | Access-Control-Allow-Origin      | 预检请求，列出正式请求中允许的域名                                                             |   `Access-Control-Allow-Origin: https://developer.mozilla.org`   |
|          | Access-Control-Expose-Headers    | 预检请求，列出正式请求中哪些首部可以暴露                                                       | `Access-Control-Expose-Headers: Content-Length, X-Kuma-Revision` |
|          | Access-Control-Max-Age           | 预检请求，列出正式请求中 Access-Control-Allow-Headers 和 Access-Control-Allow-Methods 缓存时间 |                  `Access-Control-Max-Age: 600`                   |

###### 缓存协商

| 报文形式 | 首部字段名          | 说明                                          |                         示例                         |
| :------: | ------------------- | --------------------------------------------- | :--------------------------------------------------: |
|   通用   | Cache               | 资源的缓存策略                                |              `Cache-Control: no-cache`               |
|  请求头  | If-Modified-Since   | 比较资源的更新时间                            |  `If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT`  |
|          | If-Match            | 比较实体标记 Etag                             |    `If-Match: “737060cd8c284d8af7aD3082f209582d”`    |
|          | IF-None-Match       | 比较实体标记与 If-Match 相反                  | `If-None-Match: “737060cd8c284d8af7ad3082f209582d”`  |
|          | If-Range            | 资源未更新时发送实体 Byte 的范围请求          |    `If-Range: “737060cd8c284d8af7ad3082f209582d”`    |
|          | IF-Unmodified-Since | 比较资源的更新时间，与 If-Modified-Since 相反 | `If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT` |
|  响应头  | Expires             | 表示资源的过期时间                            |       `Expires: Thu, 01 Dec 2010 16:00:00 GMT`       |
|          | Last-Modified       | 表示服务器认定资源最后的修改时间              |    `Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT`    |
|          | Etag                | 表示变量的实体标签的当前值                    |      `ETag: “737060cd8c284d8af7ad3082f209582d”`      |

###### 权限认证

| 报文形式 | 首部字段名          | 说明                                                   |                              示例                               |
| :------: | ------------------- | ------------------------------------------------------ | :-------------------------------------------------------------: |
|   通用   | Via                 | 代理服务器的相关信息                                   |          `Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)`          |
|  请求头  | Authorization       | 验证用户代理身份的凭证                                 |       `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`       |
|          | Proxy-Authorization | 代理服务器对客户端的认证信息                           | `Proxy-Authenticate: Basic realm="Access to the internal site"` |
|  响应头  | WWW-Authorization   | 服务器对客户端的认真信息                               |                    `WWW-Authenticate: Basic`                    |
|          | Proxy-Authorization | 用于指定代理服务器上的资源访问权限而采用的身份验证方式 | `Proxy-Authenticate: Basic realm="Access to the internal site"` |

###### 其他

| 首部字段名        | 说明                                                        | 示例                                           |
| ----------------- | ----------------------------------------------------------- | ---------------------------------------------- |
| Allow             | 资源可支持的 HTTP 方法                                      | `Allow: GET,HEAD`                              |
| Trailer           | 报文末端的首部一览                                          | `Trailer: Max-Forwards`                        |
| Transfer-Encoding | 指定报文主题的传输编码方式                                  | `Transfer-Encoding:chunked`                    |
| Location          | 用来重定向接收方到非请求 URL 的位置来完成请求或标志新的资源 | `Location: http://www.leixuesong.cn/724`       |
| Retry-After       | 如果实体暂时不可取，通知客户端在指定时间后在尝试            | `Retry-After: 120`                             |
| Server            | Web 服务器的安装信息                                        | `Server: Apache/1.3.27 (Unix) (Red-Hat/Linux)` |

##### HTTP 状态

**临时响应并需要请求者继续执行操作**

| 状态码 | 含义                | 说明                                                                             |
| ------ | ------------------- | -------------------------------------------------------------------------------- |
| 100    | Continue            | 继续(`请求者应当继续提出请求，服务器返回此代码表示已收到请求的第一部分正在等待`) |
| 101    | Switching Protocols | 交换协议(`请求者已要求服务器交换协议，服务器已确认准备切换`)                     |
| 102    | Processing          | 处理中(`服务器正在处理，但无响应可用`)                                           |

**请求成功的状态**

| 状态码 | 含义                          | 说明                                                                                                                                                                                                                                                                   |
| ------ | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200    | OK                            | 成功(`服务器成功处理了请求`)                                                                                                                                                                                                                                           |
| 201    | Created                       | 已创建(`请求成功并且服务器创建了新的资源`)                                                                                                                                                                                                                             |
| 202    | Accepted                      | 已接收(`服务器已接受请求，但尚未处理`)                                                                                                                                                                                                                                 |
| 203    | Non-Authoritativa Information | 非授权信息(`服务器已成功处理了请求，但返回的信息可能来自另一来源`)                                                                                                                                                                                                     |
| 204    | No Content                    | 无内容(`服务器成功处理了请求，但没有返回任何内容`)                                                                                                                                                                                                                     |
| 205    | Reset Content                 | 重置内容(`服务器成功处理了请求，但没有返回任何内容`)                                                                                                                                                                                                                   |
| 206    | Partial Content               | 部分内容(`服务器成功处理了部分GET请求，使用场景为HTTP分块下载和断电续传，当然也会带上相应的响应头Content-Range20`)                                                                                                                                                     |
| 207    | Multi-status(WebDAV)          | 多状态(`服务器返回的消息体重包含了多个状态码，用于表示一个请求对多个资源的不同操作结果，使用场景主要用于WebDAV扩展中，响应中返回一个XML消息体，其中包含了多个独立操作的状态码和描述，比如，一个请求可能涉及对多个文件的操作，服务器可以在207响应中返回每个文件的状态`) |
| 208    | Already-Peported              | 已报告(`DAV绑定的成员已经在多状态响应之前的部分被列举，且未被再次包含`)                                                                                                                                                                                                |
| 226    | IM Used                       | 使用的(`服务器已经满足了对资源的请求，对实体请求的一个或多个实体操作的结果表示`)                                                                                                                                                                                       |

重定向

| 状态码 | 含义               | 说明                                                                                                        |
| ------ | ------------------ | ----------------------------------------------------------------------------------------------------------- |
| 300    | MultipleChoices    | 多种选择(`针对请求，服务器可以执行多种操作。服务器可根据请求者选择一项操作或提供操作列表供请求者选择`)      |
| 301    | Moved Permanenly   | 永久重定向(`请求的网页已永久移动到新位置，服务器返回此响应时，会自动请求转到新地址响应头Location为新的URL`) |
| 302    | Found              | 临时重定向(`请求的网页已转移到新的URL，Location返回新的URL，但请求者后续仍然使用原来地址进行请求`)          |
| 303    | See Other          | 查看其他位置(`请求者应当对不同的位置使用单独的GET请求来检索响应时，服务器返回此代码`)                       |
| 304    | Not Modified       | 未修改(`自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容`)                          |
| 305    | UseProxy           | 使用代理(`请求者只能使用代理访问请求的网页，如果服务器返回此响应，害表示请求者应使用代理`)                  |
| 307    | Temporary Redirect | 临时重定向(`与302类似，唯一的区别是不允许将请求方法从POST改成GET`)                                          |
| 308    | Permanent Redirect | 永久重定向(请求和所有奖励的请求应使用另一个 URI 重复)                                                       |

客户端错误

| 状态码 | 含义                          | 说明                                                                             |
| ------ | ----------------------------- | -------------------------------------------------------------------------------- |
| 400    | Bad Request                   | 错误请求                                                                         |
| 401    | Unauthorized                  | 未授权                                                                           |
| 402    | Payment Required              | 需要付费                                                                         |
| 403    | Forbidden                     | 禁止访问                                                                         |
| 404    | Not Found                     | 未找到                                                                           |
| 405    | Method Not Allowed            | 不允许的方法，禁止请求中指定的方法                                               |
| 406    | Not Acceptable                | 不可接受，无法使用请求的内容特性响应请求的网页                                   |
| 407    | Proxy Authentication Required | 需要代理授权，与 401 类似，但指定请求者应该授权使用代理                          |
| 408    | Request Timeout               | 请求超时,服务器等候请求时发生超时                                                |
| 409    | Request Conflict              | 冲突，服务器在完成请求时发生冲突，服务器必须在响应中包含有关冲突的信息           |
| 410    | Gone                          | 已删除                                                                           |
| 411    | Length Required               | 需要有效长度                                                                     |
| 412    | Precondition Failed           | 未满足前提条件                                                                   |
| 413    | Payload Too Large             | 请求实体过大                                                                     |
| 414    | URI Too Long                  | （**请求的 URI 过长**）请求的 URI（通常为网址）过长，服务器无法处理。            |
| 415    | Unsupported Media Type        | （**不支持的媒体类型**）请求的格式不受请求页面的支持。                           |
| 416    | Range Not Satisfiable         | （**请求范围不符合要求**）如果页面无法提供请求的范围，则服务器会返回此状态代码。 |
| 417    | Expectation Failed            | （**未满足期望值**）服务器未满足"期望"请求标头字段的要求。                       |
| 422    | Unprocessable Entity          | **（不可处理的实体）**请求格式正确，但是由于含有语义错误，无法响应。             |

服务端错误

| 状态码  | 说明                            |                                                                                              |
| ------- | ------------------------------- | -------------------------------------------------------------------------------------------- |
| 500     | Internal Server Error           | 服务器内部错误                                                                               |
| 501     | Not Implemented                 | （**未执行**）服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。  |
| **502** | Bad Gateway                     | （**错误网关**）服务器作为网关或代理，从上游服务器收到无效响应。                             |
| 503     | Service Unavailable             | （**服务不可用**）服务器目前无法使用（由于超载或停机维护）。通常，这只是暂时状态。           |
| 504     | Gateway Timeout                 | （**网关超时**）服务器作为网关或代理，但是没有及时从上游服务器收到请求。                     |
| 505     | HTTP Version Not Supported      | （**HTTP 版本不受支持**）服务器不支持请求中所用的 HTTP 协议版本。                            |
| 506     | Variant Also Negotiates         | （**变体也进行协商**）                                                                       |
| 507     | Insufficient Storage            | （**存储空间不足**）服务器无法存储完成请求所必须的内容。                                     |
| 508     | Loop Detected                   | （**检测到循环**）服务器在处理请求时陷入死循环 3                                             |
| 509     | Bandwidth Limit Exceeded        | （**带宽限制超出**）                                                                         |
| 510     | Not Extended                    | （**未满足**）获取资源所需要的策略并没有被满足。                                             |
| 511     | Network Authentication Required | （**网络认证需要**）客户端需要进行身份验证才能获得网络访问权限，旨在限制用户群访问特定网络。 |

##### HTTP 连接

> HTTP 连接是指客户端与服务器之间通过 HTTP 协议进行通信的过程，采用的是请求-应答的模式

请求-应答模式分为两种：

- 普通模式：每个请求-应答客户和服务器都新建一个链接，完成之后立即断开
- Keep-Alive 模式：客户端到服务器的连接持续有效，避免了重新建立连接

###### HTTP 连接的建立

> HTTP 是基于 TCP 的应用层协议，通信通常通过 TCP 链接进行，当客户端向服务器发送请求时，首先建立一个 TCP 连接，这个过程也就是三次握手

![img](https://img-blog.csdnimg.cn/20200829115116478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM4MTA2OTIz,size_16,color_FFFFFF,t_70)

- **第一次握手**：客户端向服务器发送一个 TCP 的包，其中包含 SYN 的标志

  - `SYN = 1： 表示这是一个连接请求`

  - `Seq = x：客户端初始化一个序列号X，这个序列号用于标志后续传输的数据包的顺序`

    `客户端进入"SYN_SENT "状态，表示连接请求已发送，等待服务器响应`

    ```
    客户端 -> 服务器: [SYN, Seq = X]
    ```

- **第二次握手**：服务器回应 SYN-ACK

  - `SYN = 1： 表示服务器同意连接请求`
  - `Seq = Y：服务器生成一个新的初始化序列号Y，用于标识服务器的数据包顺序`
  - `ACK = x + 1: 确认号Ack表示服务器已经接收到客户端的SYN包，期望客户端下一个数据包的序列号为X + 1`

  `服务器进入”SYN_RECEIVED“等待客户端确认状态`

  ```
  服务器 -> 客户端: [SYN, ACK, Seq = Y, Ack = X + 1]
  ```

- **第三次握手**：客户端发送 ACK。客户端收到服务器的 SYN-ACK 后，发送一个包含 ACK 标志的包给服务器，确认连接建立

  - `ACK = 1： 表示对服务器SYN报的确认`

  - `Seq = X +１：表示客户端的数据包序列号为X + 1，准备开始传输数据`

  - `ACK  = Y +１：确认号ACK表示客户端已经收到服务器的SYN包，期望服务器下一个数据包的序列号为Y+ 1`

    `客户端进入”ESTABISHED“表示连接建立，可以开始传输数据`

    ```
    客户端 -> 服务器: [ACK, Seq = X + 1, Ack = Y + 1]
    ```

###### HTTP 连接的类型

- 短连接

  > 传统的 HTTP/1.0 协议使用短连接，每次请求-响应后，TCP 连接会立即关闭，

- 长连接(持久连接)

  > HTTP/1.1 引入了长连接的概念，通过在 HTTP 请求头添加 Connection: keep-alive，允许多个请求和响应在一个 TCP 中完成，减少了建立连接和开关的开销，提高了通信效率

  `使用长连接，客户端和服务器如何知道本次传输结束了?`

  1. Content-Length: 表示响应体的字节长度，当接收到的数据长度达到 Content-Length 指定的长度时，客户端就知道本次传输完成

     ```
     HTTP/1.1 200 OK
     Content-Type: text/html
     Content-Length: 1024
     <html>...</html>  (1024 字节的响应体)
     ```

     2. Transfer-Encoding: chunked

        `Transfer-Encoding会改变报文的格式和传输的方式，使用不但不会减少内容传输的大小，甚至会变大，传输编码必须配置持久连接使用，为了持久连接中，将数据分块传输，并标记传输结束`

        对于动态生成的内容在响应开始时无法确定长度，可以使用数据以一些列块的形式传输，每个块都有大小标志，最后一个块的大小为 0，表示传输结束。

###### HTTP 连接的状态

- 打开状态
- 关闭状态
- 半关闭状态

###### HTTP/2，HTTP/3，HTTPS 的连接

> HTTP2 引入了多路复用，允许在单一 TCP 连接上同时发送多个请求和响应
>
> HTTP3 基于 QUIC 协议，而不是 TCP，提供更快地连接建立和更低的延迟
>
> HTTPS 是 HTTP 的安全版本，使用 TLC 协议对数据进行加密

##### HTTP 内容协商

> 内容协商是一种机制，允许服务器和客户端在资源请求和相应的过程中协商并选择最合适的资源版本。具体来说，当客户端向服务器请求某个资源时，服务器可以根据客户端的请求头信息，返回与客户端需求最匹配的资源

实现内容协商的两种机制

- 客户端设置特定的 HTTP 头字段

  `在这种机制下，客户端在发送HTTP请求时，通过特定字段来告诉服务器它希望接受的资源版本。服务器根据头字段来决定返回最合适的资源`

  ![HTTP 内容协商机制的基本原则](https://tsejx.github.io/javascript-guidebook/static/http-content-negotiation.0d0f8e5d.png)

  ![服务端驱动型内容协商机制](https://tsejx.github.io/javascript-guidebook/static/http-content-negotiation-proactive.83206f77.png)

  | 头节点          | 字段值                                                  |
  | --------------- | ------------------------------------------------------- |
  | Accept          | 指定客户端可接受的媒体类型(text/html, application/json) |
  | Accept-Language | 指定 z 客户端首选的语言版本(en-US,zh-CN)                |
  | Accept-Encoding | 指定客户端可接受的内容编码 gzip, deflate                |
  | Accept-Charset  | 指定客户端可接受的字符集 UTF-8,ISO-8859-1               |

- 服务端返回 300 或 406 状态码

  - 300

    `返回多个可用的资源，并让客户端选择最合适的版本,这种情况通常会伴随一个HTML页面，列出所有可用资源的连接，供客户端选择`

    ```
    HTTP/1.1 300 Multiple Choices
    Content-Type: text/html

    <html>
    <body>
      <h1>Multiple Choices</h1>
      <ul>
        <li><a href="example.en.html">English Version</a></li>
        <li><a href="example.fr.html">French Version</a></li>
      </ul>
    </body>
    </html>

    ```

  - 406

    `当客户端无法根据客户端的请求头字段找到合适的资源版本时，返回此状态码，这表示服务器理解客户端的请求，但无法提供符合条件的资源`
