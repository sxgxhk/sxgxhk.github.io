---
title: Alpha SSL泛域名证书折腾记
date: 2017-06-13 12:48
tags: 随笔
categories: 我的地盘
permalink: 2476
---

主机一直用的是Oneinstack的环境包，原来想弄个SSL证书都是到处申请，后来Oneinstack升级之后，支持一键获取Let's Encryot 证书后，就一直用这个了，这不最近又想折腾点子网站，想弄SSL的，一看又得去找证书，虽然一键包支持，但好像有数量似的，弄多了就会出现不成功现象（可能是我手气不好吧），干脆去搞个泛域名证书，以后搞子网站也就不用麻烦了。


<!--more-->

![https02.jpg][1]

 - 首先去 [https://csr.chinassl.net/generator-csr.html][2]申请 CSR 文件。注意：CSR 申请那里的域名一定要填*.uefeng.com 这样的格式，要不然申请到的是单域名的。
 - 然后去 [https://assl.loovit.net/][3] 输入之前申请的 CSR 文件内容和邮箱，进入下一步会让你选择哪个管理员邮箱来接收验证邮件，点击验证连接后就能看到你的 CA 证书内容了。
 - 一切弄好之后上传服务器，并重启Nginx，小绿锁就这么轻松易主了，还是泛的哦
![https03.jpg][4]

本以为到此就OK了，可当我拿出手机想访问看时，竟然提示证书风险@(狂汗)，回想前些天在有意上看过类似的文章，赶紧去打开，原来出现这种问题是因为证书没有提供 AlphaSSL CA证书链，只要补齐了，手机浏览器就不会提示风险了，具体内容如下：
```php
-----BEGIN CERTIFICATE-----
MIIETTCCAzWgAwIBAgILBAAAAAABRE7wNjEwDQYJKoZIhvcNAQELBQAwVzELMAkGA1UEBhMCQkUx
GTAXBgNVBAoTEEdsb2JhbFNpZ24gbnYtc2ExEDAOBgNVBAsTB1Jvb3QgQ0ExGzAZBgNVBAMTEkds
b2JhbFNpZ24gUm9vdCBDQTAeFw0xNDAyMjAxMDAwMDBaFw0yNDAyMjAxMDAwMDBaMEwxCzAJBgNV
BAYTAkJFMRkwFwYDVQQKExBHbG9iYWxTaWduIG52LXNhMSIwIAYDVQQDExlBbHBoYVNTTCBDQSAt
IFNIQTI1NiAtIEcyMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2gHs5OxzYPt+j2q3
xhfjkmQy1KwA2aIPue3ua4qGypJn2XTXXUcCPI9A1p5tFM3D2ik5pw8FCmiiZhoexLKLdljlq10d
j0CzOYvvHoN9ItDjqQAu7FPPYhmFRChMwCfLew7sEGQAEKQFzKByvkFsMVtI5LHsuSPrVU3QfWJK
pbSlpFmFxSWRpv6mCZ8GEG2PgQxkQF5zAJrgLmWYVBAAcJjI4e00X9icxw3A1iNZRfz+VXqG7pRg
IvGu0eZVRvaZxRsIdF+ssGSEj4k4HKGnkCFPAm694GFn1PhChw8K98kEbSqpL+9Cpd/do1PbmB6B
+Zpye1reTz5/olig4hetZwIDAQABo4IBIzCCAR8wDgYDVR0PAQH/BAQDAgEGMBIGA1UdEwEB/wQI
MAYBAf8CAQAwHQYDVR0OBBYEFPXN1TwIUPlqTzq3l9pWg+Zp0mj3MEUGA1UdIAQ+MDwwOgYEVR0g
ADAyMDAGCCsGAQUFBwIBFiRodHRwczovL3d3dy5hbHBoYXNzbC5jb20vcmVwb3NpdG9yeS8wMwYD
VR0fBCwwKjAooCagJIYiaHR0cDovL2NybC5nbG9iYWxzaWduLm5ldC9yb290LmNybDA9BggrBgEF
BQcBAQQxMC8wLQYIKwYBBQUHMAGGIWh0dHA6Ly9vY3NwLmdsb2JhbHNpZ24uY29tL3Jvb3RyMTAf
BgNVHSMEGDAWgBRge2YaRQ2XyolQL30EzTSo//z9SzANBgkqhkiG9w0BAQsFAAOCAQEAYEBoFkfn
Fo3bXKFWKsv0XJuwHqJL9csCP/gLofKnQtS3TOvjZoDzJUN4LhsXVgdSGMvRqOzm+3M+pGKMgLTS
xRJzo9P6Aji+Yz2EuJnB8br3n8NA0VgYU8Fi3a8YQn80TsVD1XGwMADH45CuP1eGl87qDBKOInDj
ZqdUfy4oy9RU0LMeYmcI+Sfhy+NmuCQbiWqJRGXy2UzSWByMTsCVodTvZy84IOgu/5ZR8LrYPZJw
R2UcnnNytGAMXOLRc3bgr07i5TelRS+KIz6HxzDmMTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRk
nl7OedSyps9AsUSoPocZXun4IRZZUw==
-----END CERTIFICATE-----
```
再次上传，重启Nginx，再手机访问，OK了！



  [1]: https://cdn.uu126.cn/usr/uploads/2017/06/200254446.jpg
  [2]: https://csr.chinassl.net/generator-csr.html
  [3]: https://assl.loovit.net/
  [4]: https://cdn.uu126.cn/usr/uploads/2017/06/1377359312.jpg