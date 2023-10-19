## 第 6 章	HTTP首部

[toc]

本文是在《图解HTTP》一书的基础上做补充和说明

> HTTP 协议的请求和响应报文中必定包含 HTTP 首部，只是我们平时在使用 Web 的过程中感受不到它。本章我们一起来学习 HTTP 首部的结构，以及首部中各字段的用法。

##### 6.1 HTTP 报文首部

**如图为HTTP报文的结构：**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618090210267.png" alt="image-20230618090210267" style="zoom:67%;" />

HTTP 协议的请求和响应报文中必定包含 HTTP 首部。**首部内容为客户端和服务器分别处理请求和响应提供所需要的信息**。对于客户端用户来说，这些信息中的大部分内容都无须亲自查看。

**报文首部由几个字段构成**。

###### HTTP 请求报文

> 在请求中，HTTP 报文由**方法、URI、HTTP 版本、HTTP 首部字段等部分构成**。

**如下为请求报文的构成图**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618091247422.png" alt="image-20230618091247422" style="zoom:67%;" />

下面的示例是访问 `http://hackr.jp` 时，请求报文的首部信息。

```http
GET / HTTP/1.1
Host: hackr.jp
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:13.0) Gecko/20100101 Firefox/13.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*; q=0.8
Accept-Language: ja,en-us;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: keep-alive
If-Modified-Since: Fri, 31 Aug 2007 02:02:20 GMT
If-None-Match: "45bae1-16a-46d776ac"
Cache-Control: max-age=0
```

###### HTTP 响应报文

>  在响应中，HTTP 报文由 **HTTP 版本、状态码（数字和原因短语）、 HTTP 首部字段 3 部分构成**。

**如下图为响应报文的构成图**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618091631641.png" alt="image-20230618091631641" style="zoom:67%;" />

以下示例是之前请求访问 `http://hackr.jp/` 时，返回的响应报文的首部信息。

```http
HTTP/1.1 304 Not Modified
Date: Thu, 07 Jun 2012 07:21:36 GMT
Server: Apache
Connection: close
Etag: "45bae1-16a-46d776ac"
```

#### 6.2 HTTP 首部字段

##### 6.2.1 HTTP 首部字段传递重要信息

HTTP 首部字段是构成 HTTP 报文的要素之一。在客户端与服务器之间以 HTTP 协议进行通信的过程中，无论是请求还是响应都会使用首部字段，它能起到传递额外重要信息的作用。

使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容。

**如下图：首部字段内可使用的附加信息较多**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618093124419.png" alt="image-20230618093124419" style="zoom:67%;" />

##### 6.2.2 HTTP 首部字段结构

HTTP 首部字段是由首部字段名和字段值构成的，中间用冒号“:” 分 隔。

<table><tr><th>首部字段名: 字段值</th></tr></table>

例如，在 HTTP 首部中以 Content-Type 这个字段来表示报文主体的对象类型。

<table><tr><th>Content-Type: text/html</th></tr></table>

就以上述示例来看，首部字段名为 Content-Type，字符串 text/html 是 字段值。

另外，字段值对应单个 HTTP 首部字段可以有多个值，如下所示。

<table>
    <tr><th>Keep-Alive: timeout=15, max=100</th></tr>
</table>

> **若HTTP首部字段重复了会如何？**
>
> 当 HTTP 报文首部中出现了两个或两个以上具有相同首部字段名时会怎么样？这种情况在规范内尚未明确，根据浏览器内部处理逻辑的不同，结果可能并不一致。有些浏览器会优先处理第一次出现的首部字段，而有些则会优先处理最后出现的首部字段。

##### 6.2.3 4 种 HTTP 首部字段类型

HTTP 首部字段根据实际用途被分为以下 4 种类型。

###### 通用首部字段（General Header Fields）

请求报文和响应报文两方都会使用的首部。

###### 请求首部字段（Request Header Fields）

从客户端向服务器端发送请求报文时使用的首部。补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。

###### 响应首部字段（Response Header Fields）

从服务器端向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端附加额外的内容信息。

###### 实体首部字段（Entity Header Fields）

针对请求报文和响应报文的实体部分使用的首部。补充了资源内容更新时间等与实体有关的信息。

##### 6.2.4 HTTP/1.1 首部字段一览

> HTTP/1.1 规范定义了如下 47 种首部字段。

###### 通用首部字段

| 首部字段名         | 说明                       |
| ------------------ | -------------------------- |
| Cache-Control      | 控制缓存的行为             |
| Connection         | 逐跳首部、连接的管理       |
| Date               | 创建报文的日期时间         |
| Pragme             | 报文指令                   |
| Trailer            | 报文末端的首部一览         |
| Transsfer-Encoding | 指定报文主体的传输编码方式 |
| Upgrade            | 升级为其他协议             |
| Via                | 代理服务器的习惯信息       |
| Warning            | 错误通知                   |

###### 请求首部字段

| 首部字段名          | 说明                                          |
| ------------------- | --------------------------------------------- |
| Accept              | 用户代理可处理的媒体类型                      |
| Accept-Charaset     | 优先的字符集                                  |
| Accept-Encoding     | 优先的内容编码                                |
| Accept-Language     | 优先的语言（自然语言）                        |
| Authorization       | Web认证信息                                   |
| Expect              | 期待服务器的特定行为                          |
| From                | 用户的电子邮箱地址                            |
| Host                | 请求资源所在服务器                            |
| If-Match            | 比较实体标记（ETag）                          |
| If-Modified-Since   | 比较资源的更新时间                            |
| If-None-Match       | 比较实体标记（与If-Match相反）                |
| If-Range            | 资源未更新时发送实体Byte的范围请求            |
| If-Unmodified-Since | 比较资源的更新时间（与If-Modified-Since相反） |
| Max-Forwards        | 最大传输逐跳数                                |
| Proxy-Authorization | 代理服务器要求客户端的认证信息                |
| Range               | 实体的字节范围请求                            |
| Referer             | 对请求中URI的原始获取方法                     |
| TE                  | 传输编码的优先级                              |
| User-Agent          | HTTP客户端程序的信息                          |

###### 响应首部字段

| 首部字段名         | 说明                         |
| ------------------ | ---------------------------- |
| Accept-Ranges      | 是否接受字节范围请求         |
| Age                | 推算资源创建经过时间         |
| ETag               | 资源的匹配信息               |
| Location           | 令客户端重定向至指定URI      |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After        | 对再次发起请求的时机要求     |
| Server             | HTTP服务器的安装信息         |
| Vary               | 代理服务器缓存的管理信息     |
| WWW-Authenticate   | 服务器对客户端的认证信息     |

###### 实体首部字段

| 首部字段名       | 说明                         |
| ---------------- | ---------------------------- |
| Allow            | 资源可支持HTTP方法           |
| Content-Encoding | 实体主体适用的编码方式       |
| Content-Language | 实体主体的自然语言           |
| Content-Length   | 实体主体的大小（单位：字节） |
| Content-Location | 代替对应资源的URI            |
| Content-MD5      | 实体主体的报文摘要           |
| Content-Range    | 实体主体的位置范围           |
| Content-Type     | 实体主体的媒体类型           |
| Expires          | 实体主体过期的日期时间       |
| Last-Modified    | 资源的最后修改日期时间       |

##### 6.2.5 非 HTTP/1.1 首部字段

在 HTTP 协议通信交互中使用到的首部字段，不限于 RFC2616 中定义的 47 种首部字段。还有 Cookie、Set-Cookie 和 Content-Disposition 等在其他 RFC 中定义的首部字段，它们的使用频率也很高。

这些非正式的首部字段统一归纳在 RFC4229 HTTP Header Field Registrations 中。

##### 6.2.6 End-to-end 首部和 Hop-by-hop 首部

HTTP 首部字段将定义成**缓存代理**和**非缓存代理**的行为，分成 2 种类型。

###### 什么是缓存代理和非缓存代理？

HTTP 首部字段定义了缓存代理和非缓存代理的行为。

**缓存代理（Caching Proxy）**是指在客户端和服务器之间的网络中起到中间人的作用，它可以缓存已经获取过的资源，并在后续请求中直接返回缓存的资源，而不需要再次访问服务器。缓存代理可以提高响应速度，减轻服务器负载，节省带宽。

**非缓存代理（Non-Caching Proxy）**是指在客户端和服务器之间的网络中起到中间人的作用，但它不会缓存已经获取过的资源，每次请求都会直接转发给服务器，并将服务器的响应返回给客户端。非缓存代理主要用于转发请求和响应，不具备缓存功能。

**端到端首部（End-to-end Header）**

* 分在此类别中的首部会转发给请求 / 响应对应的最终接收目标，且必须保存在由**缓存生成的响应中**，另外规定它必须被转发。

**逐跳首部（Hop-by-hop Header）**

* 分在此类别中的首部只对**单次转发有效**，会因通过缓存或代理而不再转发。HTTP/1.1 和之后版本中，如果要使用 hop-by-hop 首部，需提 供 Connection 首部字段。

###### 端到端首部和逐跳首部的用处

**端到端首部的用处**

当客户端发送HTTP请求时，请求中的端到端首部字段会被一直转发到最终的服务器，并且在整个请求-响应链路中保持不变。这些端到端首部字段包含了对请求和响应的重要信息，例如Content-Type、Content-Length、Authorization等。这些字段对于服务器的处理和客户端的解析都非常重要，因此必须被转发并保留在由缓存生成的响应中。

**逐跳首部的用处**

逐跳首部字段的主要作用是**允许在传输过程中对特定的环节进行控制和扩展**。它们只对单次转发有效，通过缓存或代理时不会再次转发。这种设计提供了更大的灵活性和可扩展性，允许在HTTP协议中添加新的功能和行为。

逐跳首部字段的存在还可以防止首部字段的冲突和歧义。如果某个中间代理不理解某个首部字段，它会将该字段视为逐跳首部字段并不进行转发，而不会影响其他的中间代理或最终的接收目标

**举个例子**

举个例子来说明逐跳首部字段的用处。假设有一个新的扩展功能需要在请求-响应链路中实现，但这个功能并不适用于所有的中间代理。在这种情况下，可以将该功能相关的首部字段标记为逐跳首部字段。当请求或响应通过代理时，代理会检查Connection首部字段中的逐跳首部字段，如果存在该字段，则代理会忽略它们，不进行转发。这样，只有支持该扩展功能的代理会处理这些首部字段，而不会影响不支持该功能的代理。

**下面列举了 HTTP/1.1 中的逐跳首部字段**

除这 8 个首部字段之外， 其他所有字段都属于端到端首部。

这8个字段被定义为逐跳首部字段是因为**它们只对单次转发有效，并且通过缓存或代理时不会再次转发**。

* **Connection**

用于控制与服务器的连接管理，指示是否需要持久连接。它是一个逐跳首部字段，由于其作用只在当前传输环节有效，不会被转发。

* **Keep-Alive**

用于指示客户端或服务器希望保持持久连接。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **Proxy-Authenticate**

在代理服务器要求进行身份验证时，用于向客户端发送身份验证方法。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **Proxy-Authorization**

用于在代理服务器进行身份验证时，向代理服务器发送身份验证凭证。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **Trailer**

用于在分块传输编码中，指示在消息主体末尾包含的首部字段列表。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **TE**

用于指示可接受的传输编码方式，例如gzip、deflate等。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **Transfer-Encoding**

用于指示消息主体的传输编码方式，例如分块传输编码。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

* **Upgrade**

用于指示客户端希望升级到的其他协议。它是一个逐跳首部字段，只在当前传输环节有效，不会被转发。

#### 6.3 HTTP/1.1 通用首部字段

> 通用首部字段是指，请求报文和响应报文双方都会使用的首部。

##### 6.3.1 Cache-Control

通过指定首部字段 Cache-Control 的指令，就能操作缓存的工作机制。

**如下图所示，首部字段Cache-Control能够控制缓存的行为**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230629085759173.png" alt="image-20230629085759173" style="zoom:67%;" />

指令的参数是可选的，多个指令之间通过“,”分隔。首部字段 CacheControl 的指令可用于请求及响应时。

```markdown
Cache-Control: private, max-age=0, no-cache
```

###### Cache-Control 指令一览

可用的指令按请求和响应分类如下所示。

**缓存请求指令**

| 指令                | 参数   | 说明                                                         |
| ------------------- | ------ | ------------------------------------------------------------ |
| no-cache            | 无     | 告知缓存代理不要直接使用缓存，而是发送验证请求到服务器（强制向源服务器再次验证） |
| no-store            | 无     | 不缓存请求或响应的任何内容                                   |
| max-age = [ 秒]     | 必需   | 响应的最大Age值                                              |
| max-stale( = [ 秒]) | 可省略 | 接收已过期的响应                                             |
| min-fresh = [ 秒]   | 必需   | **期望**在指定时间内的响应仍有效                             |
| no-transform        | 无     | 代理不可更改媒体类型                                         |
| only-if-cached      | 无     | 从缓存获取资源                                               |
| cache-extension     | -      | 新指令标记**（token）**                                      |

**缓存响应指令**

| 指令             | 参数   | 说明                                            |
| ---------------- | ------ | ----------------------------------------------- |
| public           | 无     | 可向任意方提供响应的缓存                        |
| private          | 可省略 | 仅向特定用户返回响应                            |
| no-cache         | 可省略 | 缓存前必须先确认其有效性                        |
| no-store         | 无     | 不缓存请求或响应的任何内容                      |
| no-transform     | 无     | 代理不可更改媒体类型                            |
| must-revalidate  | 无     | 可缓存但必须再向源服务器进行确认                |
| proxy-revalidate | 无     | 要求中间缓存服务器对缓存的响应有效性再 进行确认 |
| max-age = [ 秒]  | 必需   | 响应的最大Age值                                 |
| s-maxage = [ 秒] | 必需   | 公共缓存服务器响应的最大Age值                   |
| cache-extension  | -      | 新指令标记（token）                             |

###### 表示是否能缓存的指令

###### public 指令

```markdown
Cache-Control: public
```

当指定使用 public 指令时，则明确表明其他用户也可利用缓存。

###### private 指令

**如图**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230629093329333.png" alt="image-20230629093329333" style="zoom:67%;" />

```markdown
Cache-Control: private
```

当指定 private 指令后，响应只以特定的用户作为对象，这与 public 指令的行为相反。

缓存服务器会对该特定用户提供资源缓存的服务，对于其他用户发送过来的请求，代理服务器则不会返回缓存。

###### no-cache 指令

> no-cache是HTTP协议中的一个缓存控制指令，用于指示缓存代理在响应资源时不直接使用缓存副本，而是要求缓存代理发送一个验证请求到原始服务器，以确定缓存的副本是否仍然有效。

**如图**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230629093523425.png" alt="image-20230629093523425" style="zoom:67%;" />

```markdown
Cache-Control: no-cache
```

使用 no-cache 指令的**目的是为了防止从缓存中返回过期的资源**。

<u>客户端发送的请求中的no-cache指令与服务器返回的响应中的no-cache指令作用不同</u>

* **客户端发送的请求中如果包含 no-cache 指令**，则表示客户端将不会接收缓存过的响应。于是，“中间”的缓存服务器必须把客户端请求转发给源服务器。

* **如果服务器返回的响应中包含 no-cache 指令**，那么缓存服务器不能对资源进行缓存。源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止其对响应资源进行缓存操作。
* 当缓存代理收到带有no-cache指令的响应时，它会将该响应保存在缓存中，但在下次请求时，会先向原始服务器发送一个条件请求。原始服务器会检查资源是否发生了变化，如果变化了，则返回新的响应；如果没有变化，则返回一个状态码为304 Not Modified的响应，告知缓存代理可以使用缓存的副本。
* 使用no-cache指令确实可以确保客户端每次都获取到最新的资源，而不是直接使用缓存的副本。这对于需要实时更新的资源（例如新闻、股票报价等）非常重要。no-cache指令还可以防止缓存代理返回过期的资源给客户端，提高了缓存的准确性。

```markdown
Cache-Control: no-cache=Location
```

由服务器返回的响应中，若报文首部字段 Cache-Control 中对 no-cache 字段名具体指定参数值，那么客户端在接收到这个被指定参数值的首部字段对应的响应报文后，就不能使用缓存。换言之，无参数值的首部字段可以使用缓存。只能在响应指令中指定该参数。

**no-cache指令的参数值是什么？**

no-cache指令的参数值可以根据具体的应用场景和需求进行自定义，没有固定的预定义参数值。在实际使用中，可以根据需要指定适合的参数值。

一些常见的自定义参数值示例包括：

1. no-cache="Location"：指示客户端不要缓存与位置相关的资源，要求每次都从服务器获取最新的位置信息。
2. no-cache="Authentication"：指示客户端不要缓存与身份验证相关的资源，要求每次都从服务器进行身份验证。
3. no-cache="Search"：指示客户端不要缓存搜索结果，要求每次都从服务器获取最新的搜索结果。

这些只是示例，实际上可以根据具体的应用场景和需求进行自定义参数值。参数值的具体含义和行为取决于应用程序的实现和解释。**重要的是，客户端和服务器之间在使用no-cache指令时需要达成一致，以确保正确的缓存行为和预期的结果**。

###### 控制可执行缓存的对象的指令

###### no-store 指令

```markdown
Cache-Control: no-store
```

当使用 no-store 指令时，暗示请求（和对应的响应）或响应中包含 机密信息。

> 从字面意思上很容易把 no-cache 误解成为不缓存，但事实上 no-cache 代表不缓存过期的资源，缓存会向源服务器进行有效期确认后处理资源，也许称为 do-notserve-from-cache-without-revalidation 更合适。no-store 才是真正地不进行缓存，请读者注意区别理解

因此，该指令规定缓存不能在本地存储请求或响应的任一部分。

###### 指定缓存期限和认证的指令

###### s-maxage 指令                                                            

```markdown
Cache-Control: s-maxage=604800（单位 ：秒）
```

s-maxage 指令的功能和 max-age 指令的相同，它们的不同点是 smaxage 指令只适用于供多位用户使用的公共缓存服务器 （一般指代理）。也就是说，对于向同一用户重复返回响应的服务器来说，这个指令没有任何作用。

另外，当使用 s-maxage 指令后，则直接忽略对 Expires 首部字段及 max-age 指令的处理。

###### max-age 指令

**如图**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230629100543517.png" alt="image-20230629100543517" style="zoom:67%;" />

```markdown
Cache-Control: max-age=604800（单位：秒）
```

当客户端发送的请求中包含 max-age 指令时，如果判定缓存资源的缓存时间数值比指定时间的数值更小，那么客户端就接收缓存的资源。 另外，当指定 max-age 值为 0，那么缓存服务器通常需要将请求转发给源服务器。 

当服务器返回的响应中包含 max-age 指令时，缓存服务器将不对资源的有效性再作确认，而 max-age 数值代表资源保存为缓存的最长时间。 

应用 HTTP/1.1 版本的缓存服务器遇到同时存在 Expires 首部字段的情况时，会优先处理 max-age 指令，而忽略掉 Expires 首部字段。而 HTTP/1.0 版本的缓存服务器的情况却相反，max-age 指令会被忽略掉。







