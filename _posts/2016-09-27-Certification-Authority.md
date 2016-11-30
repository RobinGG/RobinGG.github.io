---
layout: post
title:  Certification Authority
---

这几天查了许多关于数字证书认证的问题,记录一下

---

### 什么是数字证书

数字证书被用来证明公钥的所有权，它包含了证书公钥的信息，公钥所有者的信息以及一个数字签名。这个签名是由一个证书颁发机构签署的。如果签名有效并且签署机构是值得信任的话，那这个公钥也就是值得信任的。随后就可以通过公钥来确认通信的信息没有经过篡改。

### 数字证书的使用

![数字证书的使用](https://upload.wikimedia.org/wikipedia/commons/9/96/Usage-of-Digital-Certificate.svg)

图片来自 [wiki-pedia](https://upload.wikimedia.org/wikipedia/commons/9/96/Usage-of-Digital-Certificate.svg)

证书颁发机构（Certificate authorities，CA）是一个第三方机构，他也有公钥私钥对以及带签名的数字证书，不过 CA 的数字证书是用自己的私钥进行的签名，也就是自签名的证书。

如果我们需要生成值得信任的数字证书，需要先生成一个数字签名请求发给 CA。这个数字签名请求中需要包含我们的公钥并用私钥签名。CA 在确认无误后会用自己的私钥签名并发回签名后的数字证书。之后我们就可以用这个证书与别人通信了。

在与其他人通信时，除了通信的内容外还会附上我们的数字证书。别人会检查我们数字证书的内容，如果签名的 CA 是他所信任的 CA 的话，就会用我们的公钥进行解密。否则他就会觉得不对劲了。比如我们访问 12306 的时候 chrome 就会矜矜业业地跳出来告诉我这个网站的证书有点问题。

