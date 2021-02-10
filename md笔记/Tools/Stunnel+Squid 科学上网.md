#Squid + Stunnel实现科学上网



## 关于squid

> Squid Cache（简称为Squid）是HTTP代理服务器软件。Squid用途广泛的，可以作为缓存服务器，可以过滤流量帮助网络安全，也可以作为代理服务器链中的一环，向上级代理转发数据或直接连接互联网。Squid程序在Unix一类系统运行。由于它是开源软件，有网站修改Squid的源代码，编译为原生Windows版；用户也可在Windows里安装Cygwin，然后在Cygwin里编译Squid。
> Squid的发展历史相当悠久，功能也相当完善。除了HTTP外，对于FTP与HTTPS的支持也相当好，在3.0测试版中也支持了IPv6。但是Squid的上级代理不能使用SOCKS协议。



## 关于stunnel

> Stunnel是一个自由的跨平台软件，用于提供全局的TLS/SSL服务。
> 针对本身无法进行TLS或SSL通信的客户端及服务器，Stunnel可提供安全的加密连接。该软件可在许多操作系统下运行，包括类Unix系统，以及Windows。Stunnel依赖于某个独立的库，如OpenSSL或者SSLeay，以实现下面的TLS或SSL协议。
> Stunnel使用基于X.509数字证书的公开密钥加密算法来保证SSL的安全连接。客户端也可以选用自签名的数字证书来得到授权。
> 如果编译时与libwrap库链接，Stunnel亦可配置为proxy-firewall服务。
> Stunnel由Michal Trojnara 和 Brian Hatch负责维护，遵照GNU通用公共许可证进行发布。

本质是想实现通过此种方式翻越光大银行的网络限制；





鸣谢：

https://blog.geekerhua.com/squid/

https://www.cnblogs.com/jinhengyu/p/10258101.html