本文为解决在linux系统下Citrix Receiver安装完成后无法登录服务器的情况，windows下没有这个问题。

其中报错为无法识别安全证书。

只需两步即可解决该问题。

前提：安装了火狐浏览器

1、sudo ln -s /usr/share/ca-certificates/mozilla/* /opt/Citrix/ICAClient/keystore/cacerts

2、sudo /opt/Citrix/ICAClient/util/ctx_rehash