---
typora-copy-images-to: ./img
---

# HTTP 协议与HTTPS 协议及常见编码方式

## 1.1 HTTP 协议

### 1.1.1 简介

**超文本传输协议**（英语：**H**yper**T**ext **T**ransfer **P**rotocol，缩写：**HTTP**）是一种用于分布式、协作式和超媒体信息系统的应用层`HTTP`是万维网的数据通信的基础。

设计`HTTP`最初的目的是为了提供一种发布和接收`HTML`页面的方法。通过`HTTP`或者`HTTPS`协议请求的资源由统一资源标识符（Uniform Resource Identifiers，URI）来标识。

`HTTP`的发展是由蒂姆·伯纳斯-李于1989年在欧洲核子研究组织（`CERN`）所发起。`HTTP`的标准制定由万维网协会（World Wide Web Consortium，`W3C`）和互联网工程任务组（Internet Engineering Task Force，`IETF`）进行协调，最终发布了一系列的`RFC`，其中最著名的是1999年6月公布的 RFC 2616，定义了`HTTP`协议中现今广泛使用的一个版本——`HTTP 1.1`。

2014年12月，互联网工程任务组（`IETF`）的Hypertext Transfer Protocol Bis（`httpbis`）工作小组将`HTTP/2`标准提议递交至`IESG`进行讨论，于2015年2月17日被批准。`HTTP/2标准`于2015年5月以`RFC 7540`正式发表，取代`HTTP 1.1`成为`HTTP`的实现标准

### 1.1.2 请求方法

#### GET

GET方法请求一个指定资源的表示形式，使用GET的请求应该只被用于获取数据。

```
 GET /books/?sex=man&name=Professional HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### HEAD

HEAD方法请求一个与GET请求的响应相同的响应，但没有响应体。

```
 HEAD /admin HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### POST

POST方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用。

```
 POST / HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Content-Type: application/x-www-form-urlencoded
 Content-Length: 40
 Connection: Keep-Alive

 sex=man&name=Professional  
```

#### PUT

PUT方法用请求有效载荷替换目标资源的所有当前表示。

```
 PUT /1.txt HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### DELETE

DELETE方法删除指定的资源。

```
 DELETE /1.txt HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### TRACE

TRACE方法沿着到目标资源的路径执行一个消息环回测试。

```
 TRACE /1.txt HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### OPTIONS

OPTIONS方法用于描述目标资源的通信选项。

```
 TRACE / HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

#### CONNECT

CONNECT方法建立一个到由目标资源标识的服务器的隧道。

```
 CONNECT / HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```

### 1.1.3 请求标记头

|    协议头字段名     |                             说明                             |                             示例                             |                             状态                             |
| :-----------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|       Accept        |           能够接受的回应内容类型（Content-Types）            |                     `Accept: text/plain`                     |                             常设                             |
|   Accept-Charset    |                       能够接受的字符集                       |                   `Accept-Charset: utf-8`                    |                             常设                             |
|   Accept-Encoding   |                   能够接受的编码方式列表。                   |               `Accept-Encoding: gzip, deflate`               |                             常设                             |
|   Accept-Language   |              能够接受的回应内容的自然语言列表。              |                   `Accept-Language: en-US`                   |                             常设                             |
|   Accept-Datetime   |                能够接受的按照时间来表示的版本                |       `Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT`       |                             临时                             |
|    Authorization    |              用于超文本传输协议的认证的认证信息              |     `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`      |                             常设                             |
|    Cache-Control    | 用来指定在这次的请求/响应链中的所有缓存机制 都必须 遵守的指令 |                  `Cache-Control: no-cache`                   |                             常设                             |
|     Connection      |                该浏览器想要优先使用的连接类型                |        `Connection: keep-alive``Connection: Upgrade`         |                             常设                             |
|       Cookie        | 之前由服务器通过 Set- Cookie （下文详述）发送的一个 超文本传输协议Cookie |               `Cookie: $Version=1; Skin=new;`                |                          常设: 标准                          |
|   Content-Length    |       以 八位字节数组 （8位的字节）表示的请求体的长度        |                    `Content-Length: 348`                     |                             常设                             |
|     Content-MD5     |    请求体的内容的二进制 MD5 散列值，以 Base64 编码的结果     |           `Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==`            | 过时的[[9\]](https://zh.wikipedia.org/wiki/HTTP头字段#cite_note-9) |
|    Content-Type     |           请求体的MIME类型 （用于POST和PUT请求中）           |      `Content-Type: application/x-www-form-urlencoded`       |                             常设                             |
|        Date         | 发送该消息的日期和时间(按照 `RFC 7231` 中定义的"超文本传输协议日期"格式来发送) |            `Date: Tue, 15 Nov 1994 08:12:31 GMT`             |                             常设                             |
|       Expect        |              表明客户端要求服务器做出特定的行为              |                    `Expect: 100-continue`                    |                             常设                             |
|        From         |                  发起此请求的用户的邮件地址                  |                   `From: user@example.com`                   |                             常设                             |
|        Host         | 服务器的域名(用于虚拟主机 )，以及服务器所监听的[传输控制协议)端口号。如果所请求的端口是对应的服务的标准端口，则端口号可被省略。自超文件传输协议版本1.1（`HTTP/1.1`）开始便是必需字段。 |           `Host: zh.fsec.io:80``Host: zh.fsec.io`            |                             常设                             |
|      If-Match       | 仅当客户端提供的实体与服务器上对应的实体相匹配时，才进行对应的操作。主要作用时，用作像 `PUT` 这样的方法中，仅当从用户上次更新某个资源以来，该资源未被修改的情况下，才更新该资源。 |        `If-Match: "737060cd8c284d8af7ad3082f209582d"`        |                             常设                             |
|  If-Modified-Since  | 允许在对应的内容未被修改的情况下返回304未修改（` 304 Not Modified` ） |      `If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT`      |                             常设                             |
|    If-None-Match    | 允许在对应的内容未被修改的情况下返回304未修改（ `304 Not Modified` ），参考 超文本传输协议的实体标记 |     `If-None-Match: "737060cd8c284d8af7ad3082f209582d"`      |                             常设                             |
|      If-Range       | 如果该实体未被修改过，则向我发送我所缺少的那一个或多个部分；否则，发送整个新的实体 |        `If-Range: "737060cd8c284d8af7ad3082f209582d"`        |                             常设                             |
| If-Unmodified-Since |  仅当该实体自某个特定时间已来未被修改的情况下，才发送回应。  |     `If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT`     |                             常设                             |
|    Max-Forwards     |             限制该消息可被代理及网关转发的次数。             |                      `Max-Forwards: 10`                      |                             常设                             |
|       Origin        | 发起一个针对 跨来源资源共享 的请求（要求服务器在回应中加入一个‘访问控制-允许来源’（'`Access-Control-Allow-Origin`'）字段）。 |       `Origin: http://www.example-social-network.com`        |                          常设: 标准                          |
|       Pragma        | 与具体的实现相关，这些字段可能在请求/回应链中的任何时候产生多种效果。 |                      `Pragma: no-cache`                      |                         常设但不常用                         |
| Proxy-Authorization |                用来向代理进行认证的认证信息。                |  `Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`   |                             常设                             |
|        Range        |   仅请求某个实体的一部分。字节偏移以0开始。参见字节服务。    |                    `Range: bytes=500-999`                    |                             常设                             |
|       Referer       | 表示浏览器所访问的前一个页面，正是那个页面上的某个链接将浏览器带到了当前所请求的这个页面。 |         `Referer: http://zh.fsec.io/wiki/Main_Page`          |                             常设                             |
|         TE          | 浏览器预期接受的传输编码方式：可使用回应协议头 Transfer-Encoding 字段中的值；另外还可用"trailers"（与"分块 "传输方式相关）这个值来表明浏览器希望在最后一个尺寸为0的块之后还接收到一些额外的字段。 |                   `TE: trailers, deflate`                    |                             常设                             |
|     User-Agent      |                 浏览器的浏览器身份标识字符串                 | `User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0` |                             常设                             |
|       Upgrade       |                 要求服务器升级到另一个协议。                 |       `Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11`       |                             常设                             |
|         Via         |          向服务器告知，这个请求是由哪些代理发出的。          |        `Via: 1.0 fred, 1.1 example.com (Apache/1.1)`         |                             常设                             |
|       Warning       |     一个一般性的警告，告知，在实体内容体中可能存在错误。     |             `Warning: 199 Miscellaneous warning`             |                             常设                             |





### 1.1.4 响应状态码

##### 1xx 消息

这一类型的状态码，代表请求已被接受，需要继续处理。这类响应是临时响应，只包含状态行和某些可选的响应头信息，并以空行结束。由于HTTP/1.0协议中没有定义任何1xx状态码，所以除非在某些试验条件下，服务器禁止向此类客户端发送1xx响应。[[3\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-3) 这些状态码代表的响应都是信息性的，标示客户应该等待服务器采取进一步行动。

- 100 Continue

  服务器已经接收到请求头，并且客户端应继续发送请求主体（在需要发送身体的请求的情况下：例如，POST请求），或者如果请求已经完成，忽略这个响应。服务器必须在请求完成后向客户端发送一个最终响应。要使服务器检查请求的头部，客户端必须在其初始请求中发送`Expect: 100-continue`作为头部，并在发送正文之前接收`100 Continue`状态代码。响应代码417期望失败表示请求不应继续。

- 101 Switching Protocols

  服务器已经理解了客户端的请求，并将通过Upgrade消息头通知客户端采用不同的协议来完成这个请求。在发送完这个响应最后的空行后，服务器将会切换到在Upgrade消息头中定义的那些协议

  只有在切换新的协议更有好处的时候才应该采取类似措施。例如，切换到新的HTTP版本（如HTTP/2）比旧版本更有优势，或者切换到一个实时且同步的协议（如WebSocket）以传送利用此类特性的资源。

- 102 Processing（WebDAV RFC 2518）

  WebDAV请求可能包含许多涉及文件操作的子请求，需要很长时间才能完成请求。该代码表示服务器已经收到并正在处理请求，但无响应可用。[[5\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-RFC_2518-5)这样可以防止客户端超时，并假设请求丢失。

- 103 Early Hints

  用来在最终的HTTP消息之前返回一些响应头。

##### 2xx 成功

这一类型的状态码，代表请求已成功被服务器接收、理解、并接受。

- 200 OK

  请求已成功，请求所希望的响应头或数据体将随此响应返回。实际的响应将取决于所使用的请求方法。在GET请求中，响应将包含与请求的资源相对应的实体。在POST请求中，响应将包含描述或操作结果的实体。

- 201 Created

  请求已经被实现，而且有一个新的资源已经依据请求的需要而创建，且其URI已经随Location头信息返回。假如需要的资源无法及时创建的话，应当返回202 Accepted'。

- 202 Accepted

  服务器已接受请求，但尚未处理。最终该请求可能会也可能不会被执行，并且可能在处理发生时被禁止。

- 203 Non-Authoritative Information（自HTTP / 1.1起）

  服务器是一个转换代理服务器（transforming proxy，例如网络加速器），以`200 OK`状态码为起源，但回应了原始响应的修改版本。

- 204 No Content

  服务器成功处理了请求，没有返回任何内容。在强制门户功能中，Wi-Fi设备连接到需要进行 Web 认证的 Wi-Fi 接入点时，通过访问一个能生成 HTTP 204 响应的的网站，如果能正常收到 204 响应，则代表无需 Web 认证，否则会弹出网页浏览器界面，显示出 Web 网页认证界面用于让用户认证登录。

- 205 Reset Content

  服务器成功处理了请求，但没有返回任何内容。与204响应不同，此响应要求请求者重置文档视图。[[12\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-12)

- 206 Partial Content RFC 7233

  服务器已经成功处理了部分GET请求。类似于FlashGet或者迅雷这类的HTTP下载工具都是使用此类响应实现断点续传或者将一个大文档分解为多个下载段同时下载。

- 207 Multi-Status（WebDAV；RFC 4918

  代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。

- 208 Already Reported （WebDAV；RFC 5842）

  DAV绑定的成员已经在（多状态）响应之前的部分被列举，且未被再次包含。

- 226 IM Used （RFC 3229）

  服务器已经满足了对资源的请求，对实体请求的一个或多个实体操作的结果表示。

##### 3xx 重定向

这类状态码代表需要客户端采取进一步的操作才能完成请求。通常，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的Location域中指明。[[2\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-iana_status_codes-2)

当且仅当后续的请求所使用的方法是GET或者HEAD时，用户浏览器才可以在没有用户介入的情况下自动提交所需要的后续请求。客户端应当自动监测无限循环重定向（例如：A→B→C→……→A或A→A），因为这会导致服务器和客户端大量不必要的资源消耗。按照HTTP/1.0版规范的建议，浏览器不应自动访问超过5次的重定向。

- 300 Multiple Choices

  被请求的资源有一系列可供选择的回馈信息，每个都有自己特定的地址和浏览器驱动的商议信息。用户或浏览器能够自行选择一个首选的地址进行重定向。

  除非这是一个HEAD请求，否则该响应应当包括一个资源特性及地址的列表的实体，以便用户或浏览器从中选择最合适的重定向地址。这个实体的格式由Content-Type定义的格式所决定。浏览器可能根据响应的格式以及浏览器自身能力，自动作出最合适的选择。当然，RFC 2616规范并没有规定这样的自动选择该如何进行。

  如果服务器本身已经有了首选的回馈选择，那么在Location中应当指明这个回馈的[URI](https://zh.wikipedia.org/wiki/URI)；浏览器可能会将这个Location值作为自动重定向的地址。此外，除非额外指定，否则这个响应也是可缓存的。

- 301 Moved Permanently

  被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个URI之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。

  新的永久性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。

  如果这不是一个GET或者HEAD请求，那么浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。

  注意：对于某些使用HTTP/1.0协议的浏览器，当它们发送的POST请求得到了一个301响应的话，接下来的重定向请求将会变成GET方式。

- 302 Found

  要求客户端执行临时重定向（原始描述短语为“Moved Temporarily”）。[[19\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-RFC_1945-19)由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

  新的临时性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。

  如果这不是一个GET或者HEAD请求，那么浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。

  注意：虽然RFC 1945和RFC 2068规范不允许客户端在重定向时改变请求的方法，但是很多现存的浏览器将302响应视作为303响应，并且使用GET方式访问在Location中规定的URI，而无视原先请求的方法。因此状态码303和307被添加了进来，用以明确服务器期待客户端进行何种反应。

- 303 See Other

  对应当前请求的响应可以在另一个URI上被找到，当响应于POST（或PUT / DELETE）接收到响应时，客户端应该假定服务器已经收到数据，并且应该使用单独的GET消息发出重定向。这个方法的存在主要是为了允许由脚本激活的POST请求输出重定向到一个新的资源。这个新的URI不是原始资源的替代引用。同时，303响应禁止被缓存。当然，第二个请求（重定向）可能被缓存。

  新的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。

  注意：许多HTTP/1.1版以前的浏览器不能正确理解303状态。如果需要考虑与这些浏览器之间的互动，302状态码应该可以胜任，因为大多数的浏览器处理302响应时的方式恰恰就是上述规范要求客户端处理303响应时应当做的。

- 304 Not Modified

  表示资源在由请求头中的If-Modified-Since或If-None-Match参数指定的这一版本之后，未曾被修改。在这种情况下，由于客户端仍然具有以前下载的副本，因此不需要重新传输资源。

- 305 Use Proxy

  被请求的资源必须通过指定的代理才能被访问。Location域中将给出指定的代理所在的URI信息，接收者需要重复发送一个单独的请求，通过这个代理才能访问相应资源。只有原始服务器才能创建305响应。许多HTTP客户端（像是Mozilla）和Internet Explorer）都没有正确处理这种状态代码的响应，主要是出于安全考虑。

  注意：RFC 2068中没有明确305响应是为了重定向一个单独的请求，而且只能被原始服务器创建。忽视这些限制可能导致严重的安全后果。

- 306 Switch Proxy

  在最新版的规范中，306状态码已经不再被使用。最初是指“后续请求应使用指定的代理”。

- 307 Temporary Redirect

  在这种情况下，请求应该与另一个URI重复，但后续的请求应仍使用原始的URI。 与302相反，当重新发出原始请求时，不允许更改请求方法。 例如，应该使用另一个POST请求来重复POST请求。

- 308 Permanent Redirect (RFC 7538)

  请求和所有将来的请求应该使用另一个URI重复。 307和308重复302和301的行为，但不允许HTTP方法更改。 例如，将表单提交给永久重定向的资源可能会顺利进行

##### 4xx 客户端错误

这类的状态码代表了客户端看起来可能发生了错误，妨碍了服务器的处理。除非响应的是一个HEAD请求，否则服务器就应该返回一个解释当前错误状况的实体，以及这是临时的还是永久性的状况。这些状态码适用于任何请求方法。浏览器应当向用户显示任何包含在此类错误响应中的实体内容。

如果错误发生时客户端正在传送数据，那么使用TCP的服务器实现应当仔细确保在关闭客户端与服务器之间的连接之前，客户端已经收到了包含错误信息的[数据包。如果客户端在收到错误信息后继续向服务器发送数据，服务器的TCP栈将向客户端发送一个重置数据包，以清除该客户端所有还未识别的输入缓冲，以免这些数据被服务器上的应用程序读取并干扰后者。

- 400 Bad Request

  由于明显的客户端错误（例如，格式错误的请求语法，太大的大小，无效的请求消息或欺骗性路由请求），服务器不能或不会处理该请求。

- 401 Unauthorized RFC 7235

  参见：HTTP基本认证、HTTP摘要认证

  类似于403 Forbidden，401语义即“未认证”，即用户没有必要的凭据。该状态码表示当前请求需要用户验证。该响应必须包含一个适用于被请求资源的WWW-Authenticate信息头用以询问用户信息。客户端可以重复提交一个包含恰当的Authorization头信息的请求。如果当前请求已经包含了Authorization证书，那么401响应代表着服务器验证已经拒绝了那些证书。如果401响应包含了与前一个响应相同的身份验证询问，且浏览器已经至少尝试了一次验证，那么浏览器应当向用户展示响应中包含的实体信息，因为这个实体信息中可能包含了相关诊断信息。

  注意：当网站（通常是网站域名）禁止IP地址时，有些网站状态码显示的401，表示该特定地址被拒绝访问网站。

- 402 Payment Required

  该状态码是为了将来可能的需求而预留的。该状态码最初的意图可能被用作某种形式的数字现金或在线支付方案的一部分，但几乎没有哪家服务商使用，而且这个状态码通常不被使用。如果特定开发人员已超过请求的每日限制，Google Developers API会使用此状态码。

- 403 Forbidden

  服务器已经理解请求，但是拒绝执行它。与401响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。如果这不是一个HEAD请求，而且服务器希望能够讲清楚为何请求不能被执行，那么就应该在实体内描述拒绝的原因。当然服务器也可以返回一个404响应，假如它不希望让客户端获得任何信息。

- 404 Not Found

  请求失败，请求所希望得到的资源未被在服务器上发现，但允许用户的后续请求。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。

- 405 Method Not Allowed

  请求行中指定的请求方法不能被用于请求相应的资源。该响应必须返回一个Allow头信息用以表示出当前资源能够接受的请求方法的列表。例如，需要通过POST呈现数据的表单上的GET请求，或只读资源上的PUT请求。

  鉴于PUT，DELETE方法会对服务器上的资源进行写操作，因而绝大部分的网页服务器都不支持或者在默认配置下不允许上述请求方法，对于此类请求均会返回405错误。

- 406 Not Acceptable

  参见：内容协商

  请求的资源的内容特性无法满足请求头中的条件，因而无法生成响应实体，该请求不可接受。

  除非这是一个HEAD请求，否则该响应就应当返回一个包含可以让用户或者浏览器从中选择最合适的实体特性以及地址栏表的实体。实体的格式由Content-Type头中定义的媒体类型决定。浏览器可以根据格式及自身能力自行作出最佳选择。但是，规范中并没有定义任何作出此类自动选择的标准。

- 407 Proxy Authentication Required RFC 2617

  与401响应类似，只不过客户端必须在代理服务器上进行身份验证。代理服务器必须返回一个Proxy-Authenticate用以进行身份询问。客户端可以返回一个Proxy-Authorization信息头用以验证。

- 408 Request Timeout

  请求超时。根据HTTP规范，客户端没有在服务器预备等待的时间内完成一个请求的发送，客户端可以随时再次提交这一请求而无需进行任何更改。

- 409 Conflict

  表示因为请求存在冲突无法处理该请求，例如多个同步更新之间的编辑冲突

- 410 Gone

  表示所请求的资源不再可用。当资源被有意地删除并且资源应被清除时，应该使用这个。在收到410状态码后，用户应停止再次请求资源。但大多数服务端不会使用此状态码，而是直接使用404状态码。

- 411 Length Required

  服务器拒绝在没有定义Content-Length头的情况下接受请求。在添加了表明请求消息体长度的有效Content-Length头之后，客户端可以再次提交该请求。[[39\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-39)

- 412 Precondition Failed （RFC 7232）

  服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。这个状态码允许客户端在获取资源时在请求的元信息（请求头字段数据）中设置先决条件，以此避免该请求方法被应用到其希望的内容以外的资源上。

- 413 Request Entity Too Large （RFC 7231）

  前称“Request Entity Too Large”，表示服务器拒绝处理当前请求，因为该请求提交的实体数据大小超过了服务器愿意或者能够处理的范围。此种情况下，服务器可以关闭连接以免客户端继续发送此请求。

  如果这个状况是临时的，服务器应当返回一个Retry-After的响应头，以告知客户端可以在多少时间以后重新尝试。

- 414 Request-URI Too Long（RFC 7231）

  前称“Request-URI Too Long”，表示请求的URI长度超过了服务器能够解释的长度，因此服务器拒绝对该请求提供服务。通常将太多数据的结果编码为GET请求的查询字符串，在这种情况下，应将其转换为POST请求。这比较少见，通常的情况包括：本应使用POST方法的表单提交变成了GET方法，导致查询字符串过长。重定向URI“黑洞”，例如每次重定向把旧的URI作为新的URI的一部分，导致在若干次重定向后URI超长。客户端正在尝试利用某些服务器中存在的安全漏洞攻击服务器。这类服务器使用固定长度的缓冲读取或操作请求的URI，当GET后的参数超过某个数值后，可能会产生缓冲区溢出，导致任意代码被执行。没有此类漏洞的服务器，应当返回414状态码。

- 415 Unsupported Media Type

  对于当前请求的方法和所请求的资源，请求中提交的互联网媒体类型并不是服务器中所支持的格式，因此请求被拒绝。例如，客户端将图像上传格式为svg，但服务器要求图像使用上传格式为jpg。

- 416 Requested Range Not Satisfiable（RFC 7233）

  前称“Requested Range Not Satisfiable”。客户端已经要求文件的一部分（Byte serving），但服务器不能提供该部分。例如，如果客户端要求文件的一部分超出文件尾端。

- 417 Expectation Failed

  在请求头Expect中指定的预期内容无法被服务器满足，或者这个服务器是一个代理服显的证据证明在当前路由的下一个节点上，Expect的内容无法被满足。

- 418 I'm a teapot

  本操作码是在1998年作为IETF的传统愚人节笑话, 在RFC 2324[超文本咖啡壶控制协议中定义的，并不需要在真实的HTTP服务器中定义。当一个控制茶壶的HTCPCP收到BREW或POST指令要求其煮咖啡时应当回传此错误。这个HTTP状态码在某些网站（包括Google.com）与项目（如Node.js、ASP.NET])和[Go语言）中用作彩蛋

- 421 Misdirected Request 

  该请求针对的是无法产生响应的服务器（例如因为连接重用）。

- 422 Unprocessable Entity

  请求格式正确，但是由于含有语义错误，无法响应

- 423 Locked

  当前资源被锁定。

- 424 Failed Dependency

  由于之前的某个请求发生的错误，导致当前请求失败，例如PROPPATCH。

- 425 Too Early 

  服务器拒绝处理在Early Data中的请求，以规避可能的重放攻击

- 426 Upgrade Required

  客户端应切换到Upgrade头字段中给出的不同协议，如TLS/1.0

- 428 Precondition Required 

  原服务器要求该请求满足一定条件。这是为了防止“未更新”问题，即客户端读取（GET）一个资源的状态，更改它，并将它写（PUT）回服务器，但这期间第三方已经在服务器上更改了该资源的状态，因此导致了冲突。”

- 429 Too Many Requests 

  用户在给定的时间内发送了太多的请求。旨在用于网络限速

- 431 Request Header Fields Too Large 

  服务器不愿处理请求，因为一个或多个头字段过大。

- 440 Login Time-out

  客户端session超时失效，需要重新登录。

- 451 Unavailable For Legal Reasons



##### 5xx 服务器错误

表示服务器无法完成明显有效的请求。这类状态码代表了服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。除非这是一个HEAD请求，否则服务器应当包含一个解释当前错误状态以及这个状况是临时的还是永久的解释信息实体。浏览器应当向用户展示任何在当前响应中被包含的实体。这些状态码适用于任何响应方法。

- 500 Internal Server Error

  通用错误消息，服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。没有给出具体错误信息。

- 501 Not Implemented

  服务器不支持当前请求所需要的某个功能。当服务器无法识别请求的方法，并且无法支持其对任何资源的请求。（例如，网络服务API的新功能）

- 502 Bad Gateway

  作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。

- 503 Service Unavailable

  由于临时的服务器维护或者过载，服务器当前无法处理请求。这个状况是暂时的，并且将在一段时间以后恢复。如果能够预计延迟时间，那么响应中可以包含一个Retry-After头用以标明这个延迟时间。如果没有给出这个Retry-After信息，那么客户端应当以处理500响应的方式处理它。

- 504 Gateway Timeout

  作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI标识出的服务器，例如HTTP、FTP、LDAP）或者辅助服务器（例如DNS）收到响应。

  注意：某些代理服务器在DNS查询超时时会返回400或者500错误。

- 505 HTTP Version Not Supported

  服务器不支持，或者拒绝支持在请求中使用的HTTP版本。这暗示着服务器不能或不愿使用与客户端相同的版本。响应中应当包含一个描述了为何版本不被支持以及服务器支持哪些协议的实体。

- 506 Variant Also Negotiates

  由《透明内容协商协议》扩展，代表服务器存在内部配置错误，[[64\]](https://zh.wikipedia.org/wiki/HTTP状态码#cite_note-RFC_2295-64)被请求的协商变元资源被配置为在透明内容协商中使用自己，因此在一个协商处理中不是一个合适的重点。

- 507 Insufficient Storage

  服务器无法存储完成请求所必须的内容。这个状况被认为是临时的。

- 508 Loop Detected 

  服务器在处理请求时陷入死循环。 （可代替 208状态码]）

- 510 Not Extended

  获取资源所需要的策略并没有被满足

- 511 Network Authentication Required ）

  客户端需要进行身份验证才能获得网络访问权限，旨在限制用户群访问特定网络。（例如连接WiFi热点时的强制网络门户）

### 1.1.5 响应标记头

|                     字段名                      |                             说明                             |                             例子                             |                            状态                            |
| :---------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :--------------------------------------------------------: |
|           Access-Control-Allow-Origin           |           指定哪些网站可参与到跨来源资源共享过程中           |               `Access-Control-Allow-Origin: *`               |                            临时                            |
|                  Accept-Patch                   |                指定服务器支持的文件格式类型。                |          `Accept-Patch: text/example;charset=utf-8`          |                            常设                            |
|                  Accept-Ranges                  |             这个服务器支持哪些种类的部分内容范围             |                    `Accept-Ranges: bytes`                    |                            常设                            |
|                       Age                       |          这个对象在代理缓存中存在的时间，以秒为单位          |                          `Age: 12`                           |                            常设                            |
|                      Allow                      |    对于特定资源有效的动作。针对HTTP/405这一错误代码而使用    |                      `Allow: GET, HEAD`                      |                            常设                            |
|                  Cache-Control                  | 向从服务器直到客户端在内的所有缓存机制告知，它们是否可以缓存这个对象。其单位为秒 |                `Cache-Control: max-age=3600`                 |                            常设                            |
|                   Connection                    |                    针对该连接所预期的选项                    |                     `Connection: close`                      |                            常设                            |
|               Content-Disposition               | 一个可以让客户端下载文件并建议文件名的头部。文件名需要用双引号包裹。 |   `Content-Disposition: attachment; filename="fname.ext"`    |                            常设                            |
|                Content-Encoding                 |      在数据上使用的编码类型。参考 超文本传输协议压缩 。      |                   `Content-Encoding: gzip`                   |                            常设                            |
|                Content-Language                 |                       内容所使用的语言                       |                    `Content-Language: da`                    |                            常设                            |
|                 Content-Length                  |       回应消息体的长度，以 字节 （8位为一字节）为单位        |                    `Content-Length: 348`                     |                            常设                            |
|                Content-Location                 |                  所返回的数据的一个候选位置                  |                `Content-Location: /index.htm`                |                            常设                            |
|                   Content-MD5                   |        回应内容的二进制 MD5 散列，以 Base64 方式编码         |           `Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==`            |                           过时的                           |
|                  Content-Range                  |           这条部分消息是属于某条完整消息的哪个部分           |           `Content-Range: bytes 21010-47021/47022`           |                            常设                            |
|                  Content-Type                   |                      当前内容的MIME类型                      |           `Content-Type: text/html; charset=utf-8`           |                            常设                            |
|                      Date                       | 此条消息被发送时的日期和时间(按照 RFC 7231 中定义的“超文本传输协议日期”格式来表示) |            `Date: Tue, 15 Nov 1994 08:12:31 GMT`             |                            常设                            |
| [ETag](https://zh.wikipedia.org/wiki/HTTP_ETag) | 对于某个资源的某个特定版本的一个标识符，通常是一个 消息散列  |          `ETag: "737060cd8c284d8af7ad3082f209582d"`          |                            常设                            |
|                     Expires                     |      指定一个日期/时间，超过该时间则认为此回应已经过期       |           `Expires: Thu, 01 Dec 1994 16:00:00 GMT`           |                         常设: 标准                         |
|                  Last-Modified                  | 所请求的对象的最后修改日期(按照 RFC 7231 中定义的“超文本传输协议日期”格式来表示) |        `Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT`        |                            常设                            |
|                      Link                       | 用来表达与另一个资源之间的类型关系，此处所说的类型关系是在 RFC 5988 中定义的 |               `Link: </feed>; rel="alternate"`               |                            常设                            |
|                    Location                     |       用来 进行重定向，或者在创建了某个新资源时使用。        |      `Location: http://www.w3.org/pub/WWW/People.html`       |                            常设                            |
|                       P3P                       | 用于支持设置P3P策略，标准格式为“`P3P:CP="your_compact_policy"`”。然而P3P规范并不成功，[[33\]](https://zh.wikipedia.org/wiki/HTTP头字段#cite_note-33)大部分现代浏览器没有完整实现该功能，而大量网站也将该值设为假值，从而足以用来欺骗浏览器的P3P插件功能并授权给第三方Cookies。 | `P3P: CP="This is not a P3P policy! ``See http://www.google.com/support/accounts/bin/answer.py?hl=en&answer=151657 for more info."` |                            常设                            |
|                     Pragma                      | 与具体的实现相关，这些字段可能在请求/回应链中的任何时候产生多种效果。 |                      `Pragma: no-cache`                      |                            常设                            |
|               Proxy-Authenticate                |              要求在访问代理时提供身份认证信息。              |                 `Proxy-Authenticate: Basic`                  |                            常设                            |
|                 Public-Key-Pins                 | 用于缓解中间人攻击，声明网站认证使用的传输层安全协议证书的散列值 | `Public-Key-Pins: max-age=2592000; pin-sha256="E9CZ9INDbd+2eRQozYqqbQ2yXLVKB9+xcprMF+44U1g=";` |                            常设                            |
|                     Refresh                     | 用于设定可定时的重定向跳转。右边例子设定了5秒后跳转至“`http://www.w3.org/pub/WWW/People.html`”。 |   `Refresh: 5; url=http://www.w3.org/pub/WWW/People.html`    | 专利并非标准Netscape实现的扩展，但大部分网页浏览器也支持。 |
|                   Retry-After                   | 如果某个实体临时不可用，则，此协议头用来告知客户端日后重试。其值可以是一个特定的时间段(以秒为单位)或一个超文本传输协议日期。 | Example 1: `Retry-After: 120`Example 2: `Retry-After: Fri, 07 Nov 2014 23:59:59 GMT` |                            常设                            |
|                     Server                      |                         服务器的名字                         |                `Server: Apache/2.4.1 (Unix)`                 |                            常设                            |
|                   Set-Cookie                    |                         HTTP cookie                          |    `Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1`     |                         常设: 标准                         |
|                     Status                      | 通用网关接口 协议头字段，用来说明当前这个超文本传输协议回应的 状态 。普通的超文本传输协议回应，会使用单独的“状态行”（"Status-Line"）作为替代，这一点是在 RFC 7230 中定义的。 |                       `Status: 200 OK`                       |           Not listed as a registered field name            |
|            Strict-Transport-Security            | HTTP 严格传输安全这一头部告知客户端缓存这一强制 HTTPS 策略的时间，以及这一策略是否适用于其子域名。 | `Strict-Transport-Security: max-age=16070400; includeSubDomains` |                         常设: 标准                         |
|                     Trailer                     |  这个头部数值指示了在这一系列头部信息由由分块传输编码编码。  |                   `Trailer: Max-Forwards`                    |                            常设                            |
|                Transfer-Encoding                | 用来将实体安全地传输给用户的编码形式。当前定义的方法包括：分块（chunked）、compress、deflate、gzip和identity。 |                 `Transfer-Encoding: chunked`                 |                            常设                            |
|                     Upgrade                     |                 要求客户端升级到另一个协议。                 |       `Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11`       |                            常设                            |
|                      Vary                       | 告知下游的代理服务器，应当如何对未来的请求协议头进行匹配，以决定是否可使用已缓存的回应内容而不是重新从原始服务器请求新的内容。 |                          `Vary: *`                           |                            常设                            |
|                       Via                       |    告知代理服务器的客户端，当前回应是通过什么途径发送的。    |        `Via: 1.0 fred, 1.1 example.com (Apache/1.1)`         |                            常设                            |
|                     Warning                     |        一般性的警告，告知在实体内容体中可能存在错误。        |             `Warning: 199 Miscellaneous warning`             |                            常设                            |
|                WWW-Authenticate                 |         表明在请求获取这个实体时应当使用的认证模式。         |                  `WWW-Authenticate: Basic`                   |                            常设                            |
|                 X-Frame-Options                 | 点击劫持保护：`deny`：该页面不允许在 frame 中展示，即使是同域名内。`sameorigin`：该页面允许同域名内在 frame 中展示。`allow-from *uri*`：该页面允许在指定uri的 frame 中展示。`allowall`：允许任意位置的frame显示，非标准值。 |                   `X-Frame-Options: deny`                    |                           过时的                           |

## 1.2 HTTPS 协议

### 1.2.1 简介

**超文本传输安全协议**（英语：**H**yper**T**ext **T**ransfer **P**rotocol **S**ecure，缩写：**HTTPS**；常称为`HTTP over TLS`、HTTP over SSL或HTTP Secure）是一种通过计算机网络进行安全通信传输协议。`HTTPS`经由`HTTP`进行通信，但利用`SSL/TLS`来加密数据包。`HTTPS`开发的主要目的，是提供对网服务器的身份认证，保护交换资料的隐私与完整性。这个协议由网景公司（`Netscape`）在1994年首次提出，随后扩展到互联网上。

历史上，`HTTPS`连接经常用于万维网上的交易支付和企业信息系统中敏感信息的传输。在2000年代末至2010年代初，`HTTPS`开始广泛使用，以确保各类型的网页真实，保护账户和保持用户通信，身份和网络浏览的私密性。

另外，还有一种安全超文本传输协议（`S-HTTP`）的`HTTP`安全传输实现，但是`HTTPS`的广泛应用而成为事实上的`HTTP`安全传输实现，`S-HTTP`并没有得到广泛支持。

### 1.2.2 认证方式

![image-20220409202913534](../../images/image-20220409202913534-9507355.png)



## 1.3 常见编码方式

### 1.3.1 URL 编码

#### 介绍

百分号编码（英语：`Percent-encoding`），又称：`URL`编码（`URL encoding`）是特定上下文的统一资源定位符 （`URL`）的编码机制，实际上也适用于统一资源标志符（`URI`）的编码。也用于为 `application/x-www-form-urlencoded MIME`准备数据，因为它用于通过`HTTP`的请求操作（`request`）提交`HTML`表单数据。

#### 加解密

加密

```
https://www.bejson.com/enc/urlencode/
```

![image-20220408203907854](../../images/image-20220408203907854.png)

解密

![image-20220408203932749](../../images/image-20220408203932749.png)

### 1.3.2 HTML 编码

#### 介绍

在编写`HTML`的时候，可能会出现我们想要页面输出的内容恰好和`HTML`语法的规则冲突，导致该显示的文本内容被浏览器当作了标签，进而被构造成了页面结构。因此产生了实体字符这个东西，当浏览器解析到实体字符以后，就不会把它当成页面结构，而是转换成对应的字符展示到页面上。

而实体编号是`&`#`+`字符的`ascii`;构成。作用和实体字符相同，也可以用来帮助浏览器识别预留字符。
#### 加解密

```
https://www.convertstring.com/zh_CN/EncodeDecode/HtmlEncode
```

![image-20220408205524324](../../images/image-20220408205524324.png)

解密

![image-20220408210106534](../../images/image-20220408210106534.png)

### 1.3.3 BASE64 编码

#### 介绍

`Base64`是网络上最常见的用于传输`8Bit`字节码的编码方式之一，`Base64`就是一种基于`64`个可打印字符来表示二进制数据的方法。可查看`RFC2045～RFC2049`，上面有`MIME`的详细规范。
`Base64`编码是从二进制到字符的过程，可用于在`HTTP`环境下传递较长的标识信息。采用`Base64`编码具有不可读性，需要解码后才能阅读。
`Base64`由于以上优点被广泛应用于计算机的各个领域，然而由于输出内容中包括两个以上“符号类”字符`（+, /, =)`，不同的应用场景又分别研制了`Base64`的各种“变种”。为统一和规范化`Base64`的输出，`Base62x`被视为无符号化的改进版本。

#### 加解密

```
https://www.matools.com/base64
```

![image-20220408162841357](../../images/image-20220408162841357.png)

解码

```
https://www.matools.com/base64
```

![image-20220408162932563](../../images/image-20220408162932563.png)

### 1.3.4 MD5 编码

#### 介绍

`MD5`消息摘要算法（英语：`MD5 Message-Digest Algorithm`），一种被广泛使用的密码散列函数，可以产生出一个`128`位（`16`个字符(`BYTES`)）的散列值（`hash value`），用于确保信息传输完整一致。`MD5`由美国密码学家罗纳德·李维斯特（`Ronald Linn Rivest`）设计，于`1992`年公开，用以取代`MD4`算法。这套算法的程序在` RFC 1321 `中被加以规范。

将数据（如一段文字）运算变为另一固定长度值，是散列算法的基础原理。

1996年后被证实存在弱点，可以被加以破解，对于需要高度安全性的资料，专家一般建议改用其他算法，如SHA-2。2004年，证实MD5算法无法防止碰撞攻击，因此不适用于安全性认证，如SSL公开密钥认证或是数字签名等用途。

#### 加解密

编码

`https://www.cmd5.com/`

![image-20220408162605294](../../images/image-20220408162605294-9406368.png)

解码

`https://www.cmd5.com/`

![image-20220408162654235](../../images/image-20220408162654235.png)

### 1.3.5 ASCII 编码

#### 介绍

`ASCII` (`American Standard Code for Information Interchange`，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统。它主要用于显示现代英语，而其扩展版本延伸美国标准信息交换码则可以部分支持其他西欧语言，并等同于国际标准`ISO/IEC 646`。

`1968`年版`ASCI`I编码速见表
美国信息交换标准代码是这套编码系统的传统命名，互联网号码分配局现在更倾向于使用它的新名字`US-ASCII`。

美国信息交换标准代码是美国电气和电子工程师协会里程碑之一。

`ASCII `由电报码发展而来。第一版标准发布于`1963`年，`1967`年经历了一次主要修订，最后一次更新则是在`1986`年，至今为止共定义了`128`个字符；其中`33`个字符无法显示（一些终端提供了扩展，使得这些字符可显示为诸如笑脸、扑克牌花式等`8-bit`符号），且这33个字符多数都已是陈废的控制字符。控制字符的用途主要是用来操控已经处理过的文字。在`33`个字符之外的是`95`个可显示的字符。用键盘敲下空白键所产生的空白字符也算1个可显示字符（显示为空白）。

#### 加解密

`https://www.matools.com/code-convert-ascii`

![image-20220408163114279](../../images/image-20220408163114279.png)

解码

```
https://www.matools.com/code-convert-ascii
```

![image-20220408163209612](../../images/image-20220408163209612.png)

### 1.4 常见的数据交换格式

#### 1.4.1 JSON

#### 介绍

`JSON`（`JavaScript Object Notation`）是由道格拉斯·克罗克福特构想和设计的一种轻量级资料交换格式。其内容由属性和值所组成，因此也有易于阅读和处理的优势。`JSON`是独立于编程语言的资料格式，其不仅是`JavaScript`的子集，也采用了`C`语言家族的习惯用法，目前也有许多编程语言都能够将其解析和字符串化，其广泛使用的程度也使其成为通用的资料格式。

#### 格式

```
{
          "age": "20",
          "name": "张三",
          "sex": "男"
}
```

#### 1.4.2 XML

#### 介绍

可扩展标记语言（英语：`Extensible Markup Language`，简称：`XML`）是一种标记语言。`XML`是从标准通用标记语言（`SGML`）中简化修改出来的。它主要用到的有可扩展标记语言、可扩展样式语言（`XSL`）、`XBRL`和`XPath`等。

#### 格式

```
<?xml version="1.0" encoding="UTF-8"?>
<o>
     <age type="string">20</age>
     <name type="string">张三</name>
     <sex type="string">男</sex>
</o>

```

