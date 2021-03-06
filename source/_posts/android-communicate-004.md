---

title: 通讯篇 第四章 协议与HTTPS
date: 2019-12-3 10:38:36
author: Maizi
categories: Android - 01 - 初级开发

---

在Android开发中网络请求是最常见的操作之一，并且Android SDK中对HTTP也提供了很好的支持：`HttpClient、HttpURLConnection`。

当然也有一些很好用的网络开源框架，例如：`OkHttp和Retrofit`等，相信对于每一个开发者而言，这些都不会陌生，而本章就让我门来了解一下这些框架背后的故事。

**上述框架的介绍可以参考：**

[通讯篇 第三章 网络通信](https://idailybread.github.io/android-communicate-003/)

## 协议

### 简介

所谓协议，就是一种规则，是浏览器和服务器之间进行沟通的一种规范，学习网络通信，便是从了解这些规范开始的。

互联网的本质其实就是一系列网络协议，这个协议叫做`OSI协议`。根据不同的功能，它将网络通信的工作分为7层：

![](/img/android/20191124131231.jpg)

当然，根据使用场景的不同，也可能将它分为5层，甚至是4层。

我们所熟知的`TCP/IP`的体系结构便是4层结构：

![](/img/android/20191124142041.jpg)

`TCP/IP`（Transmission Control Protocol/Internet Protocol，传输控制协议/网际协议）是指能够在多个不同网络间实现信息传输的协议簇。TCP/IP协议不仅仅指的是TCP和IP两个协议，而是指一个由FTP、SMTP、TCP、UDP、IP等协议构成的协议簇， 只是因为在TCP/IP协议中TCP协议和IP协议最具代表性，所以被称为TCP/IP协议。

相对于OSI协议，`TCP/IP`的使用更为广泛和实用。OSI虽然概念清楚，但表示层和会话层对于大部分应用来说都是冗余的，分层过于复杂。

### Socket

Socket，即套接字接口，本身并不是协议而是接口，它是对TCP/IP协议的封装，通过Socket，我们才能使用TCP/IP协议。

进行数据通信时，TCP不可避免的会遇到同时为多个程序提供并发服务的情况。多个TCP连接同一个TCP协议端口传输数据。为了将它们区分开，便有了套接字接口Socket。

建立Socket连接至少需要一对套接字，其中一个运行于客户端，称为ClientSocket，另一个运行于服务器端，称为ServerSocket。

套接字之间的连接过程分为三个步骤：服务器监听，客户端请求，连接确认。

创建Socket连接时，可以指定使用的传输层协议，比如指定TCP协议进行连接时，该Socket连接就是一个TCP连接。

当然也有些协议并不进行连接，比如UDP，UDP传送数据前并不与对方建立连接，对接收到的数据也不发送确认信号，发送端不知道数据是否会正确接收，当然也不用重发。

### 必要性

为什么要有这些协议？

就像来自不同地域的两个人，只有掌握了同一种语言，才有实现正常交流的可能性。

相应的，网络通信也是如此。通过协议，使得那些由不同厂商制作的设备、CPU、操作系统等组成的计算机之间，只要遵循相同的协议就可以实现通信。

## HTTP

### 简介

`HTTP`(Hyper Text Transfer Protocol)便是`TCP/IP`协议的一个`应用层协议`，它用于定义WEB浏览器与WEB服务器之间交换数据的过程及数据本身的格式，是互联网上应用最为广泛的一种网络协议。

### 格式

* 请求

```
- 请求行
- 请求头
- 空行
- 请求体
```

* 响应

```
- 响应行：
- 响应头：
- 空行
- 响应体
```

请求方式有：`Get、POST、HEAD、PUT、DELETE、CONNECT、TRACE、OPTIONS、Patch`等。

**有关HTTP的介绍可以参考：**

[通讯篇 第三章 网络通信](https://idailybread.github.io/android-communicate-003/)

## HTTPS

### 简介

HTTP虽然应用广泛，但并不安全，HTTP协议传输的数据都是未加密的，也就是明文的，很容易被中间人截获或攻击。并且随着科技的发展，人们技术水平的提高，网络基础知识的普及，导致这种攻击的门槛和成本越来越低。

即便是一个不怎么懂计算机的人，花几个小时学习一下`charles`的使用，也能很快做到。

对此，HTTPS这种附带加密算法的协议就诞生了。

相对于HTTP，他在传输层和应用层之间增加了一层网络传输安全协议层（SSL/TLS协议），来保证数据传输的安全性与TCP连接建立的安全性。可以极大增加第三者攻击的成本。

### 发展历史

SSL（安全套接字层）协议，TLS（安全传输层）协议，都属于加密协议，`其中，TLS协议是SSL协议的升级版`。

* SSL（安全套接字层）协议

```
1994年Netscape公司设计了SSL 1.0，但是未发布
1995年Netscape公司发布SSL 2.0，存在严重的安全漏洞被3.0替代
1996年Netscape发布SSL 3.0，得到大规模应用
```

* TLS（安全传输层）协议

```
1999年IETF将SSL标准化，并称作TLS，发布了SSL的升级版TLS 1.0版本
2006年发布TLS 1.1
2008年发布TLS 1.2，目前应用最广泛的版本
2018年8月发布TLS 1.3版本
```

TLS在SSL v3.0的基础上，主要增加了以下内容：

　　1）更安全的MAC算法

　　2）更严密的警报

　　3）“灰色区域”规范的更明确的定义

### 加密方式

在SSL协议中，使用了非对称加密和对称加密，首先介绍一下两种加密方式的基本概念。

<b>`非对称加密`</b>：也称公开密钥加密，加解密采用一对密钥，公钥公开，私钥保密。缺点是计算耗时，`主要用于身份认证`，通常以数字证书的形式存在。

常见的加密算法有：RSA、ElGamal、背包算法、Rabin、迪菲-赫尔曼密钥交换协议中的公钥加密算法、椭圆曲线加密算法（ECC）。

<b>`对称加密`</b>：加解密采用采用相同的密钥，因此是对称的，且在通信双方共享，它的加解密速度比非对称加密快很多，因此在频繁的内容传输过程中，`使用对称加密，来保证数据隐私`。

常见的加密算法有：DES、AES、3DES、Blowfish、IDEA、RC5、RC6。

MD5并不属于非对称加密或者对称加密。

Base64也不算安全领域的加密算法，只能算是一个编码算法，对数据内容进行编码来适合传输。

**为什么需要使用两种加密方式呢？**

`仅使用对称加密：`在互联网中，通信的双方大多是临时建立的连接，互不相识，所以很难提前协商好密钥。

`仅使用非对称加密：`比如有一对密钥，公钥公开，A保留私钥，B使用公钥加密信息发送给A时，A可以使用私钥进行解密。即使C截获了B给A发送的信息，因为没有私钥的原因，C也无法读取。但是当A向B发送消息时，因为公钥匙公开的缘故，C也可以使用公钥读取信息的具体内容。

**有关加密算法的介绍可以参考：**

[安全篇 第一章 数据加密](https://idailybread.github.io/android-safety-001/)

### 加密流程

基于以上的原因，在建立连接时，客户端会生成一个随机密钥，用服务器的公钥对这个密钥进行非对称加密，服务器用私钥进行解密，这样，通信双方就得到了一个不为第三者知道的随机密钥。

然后双方就用这个对称密钥来进行数据加密。

即保证了密钥本身的安全性，也保证了数据的隐私。

便于理解，具体的加密流程可以抽象成（实际情况要比这复杂）：

![](/img/android/20191130183025.jpg)

* A通过公钥加密把随机数1发送给B，B通过私钥解密获取随机数1
* B通过私钥加密把随机数2发送给A，A通过公钥解密获取随机数2
* A通过公钥加密把随机数3发送给B，B通过私钥解密获取随机数3
* 这样双方都得到了123，然后分别使用已知算法PRF（增强的伪随机功能）计算生成密钥k
* 这样就可以通过密钥k愉快的进行数据传输了，即使C截获了2，也无法得到最终的密钥k

### 校验证书

通过上述方式，是否就能100%保证数据的安全呢？

其实并没有，世界上没有绝对安全的方式。有人能发明出无懈可击的盾，就有人能创造出无坚不摧的矛。我们所做的，只不过是把围墙铸得更高，使它能够抵御更强的破坏。不断的提升攻击它的成本和门槛。

而现在的围墙还不够高，我们需要让他更高一点。

通过截取信息的方式，C无法获取到信息的具体内容。但是，在互联网中，A和B在通信之前，其实是互不相识的两个人。

既不认识，就无法验证身份的真伪。C可以生成自己的私钥和公钥，然后伪装成B和A进行通信，获取真实信息之后，然后伪装成A和B进行通信。

因此，想要互不相识的两个人也能辨认双方的身份，就需要一个双方都信任的机构，然后制作一个双方都承认的身份证明，这就有了证书。它是由权威机构（CA）颁发的。这样，A在发送信息给B时，不再是单纯的通过公钥加密的数据，而是由CA认证的证书。这样B在收到A的数据时，就能辨认A的身份。

CA机构的全称为Certificate Authority 证书认证中心。只有通过WebTrust 国际安全审计认证，根证书才能预装到主流浏览器，成为全球可信的SSL证书颁发机构。

**证书的主要内容有：**

* 颁发机构(CA)的名字(DigiCert、Comodo等)
* 证书持有者
* 证书持有者的公钥和算法
* 证书有效期
* 证书内容本身的数字签名和算法
* 证书唯一序列号

**验证证书流程：**

* A想要和B进行通信，但不确认B的身份，于是B把自己的证书通过私钥加密给A
* A收到证书之后，使用本地配置的权威机构的公钥对证书进行解密，得到服务端的公钥和证书的数字签名，数字签名经过CA公钥解密得到证书信息摘要
* 然后用证书签名的方法计算一下当前证书的信息摘要，与收到的信息摘要作对比，如果一样，便能确认B的身份

虽然C能拿到证书内容并修改，但是因为没有权威机构的私钥，无法对证书重新加密，也就无法伪装B的身份。而如果去CA那边寻求认证，则必须提供例如域名的whois信息、域名管理邮箱等证明你是B身份的东西。

### 整体流程

了解了事情的起因经过，那么我们来总结一下结果：

* 客户端向服务器发送请求，例如https://baidu.com，并连接到服务器的443端口。发送随机值1和自己支持的加密算法。
* 服务器接到消息之后，协商好双方都能接受的加密算法，并将加密算法和随机值2发送给客户端。
* 然后将自己的数字证书发送给客户端。
* 客户端收到证书之后解析，确认服务器身份，发送由随机值1和2以及预主密钥3生成的会话密钥k。
* 服务器获取得到会话密钥k，跟客户端的一致。
* 双方通过密钥k加密信息，并读取信息。

![](/img/android/20191201221418.jpg)

当然这里使用的是单向认证，通常情况下，客户端作为发送者可能比接受者更加关心对方的身份。

双向认证的原理和单向认证基本一致，只是除了客户端认证服务器外，服务器也会认证客户端的身份。

### 握手和挥手

大家可能听过TCP三次握手或者TCP四次挥手之类的，这里也介绍一下，`TCP通过三次握手和服务器建立连接，通过四次挥手释放连接`。

为什么需要多次握手和挥手，是因为感情深吗？

简单来说是为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。

假如只有一次握手，客户端在发送第一个连接请求的报文段并没有丢失，但是在某个网络节点滞留了，导致到达服务器的时间延误了很多。本来这个请求已经失效，但是服务器并不知道，在收到这个连接请求时，误以为是一个新的请求，于是就向客户端确认请求，建立连接。但因为是一个失效的请求，客户端并不会处理，也不会发送数据。但是连接建立以后，服务器则一直在等待数据返回，这样就造成了服务器资源的浪费。

虽然说网络的不安全不稳定特性决定了多少次握手都不能保证连接的可靠性，但TCP的三次握手在最低限度上(实际上也很大程度上保证了)保证了连接的可靠性。

TCP三次握手和TCP四次挥手流程：

SYN表示建立连接，ACK表示响应，FIN表示关闭连接。

![](/img/android/20191201230900.jpg)