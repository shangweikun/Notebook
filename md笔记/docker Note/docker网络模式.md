原理：

### Linux bridge 模式

Linux bridge 模式下，Linux Kernel 会创建出一个**虚拟网桥** ，用以实现**主机网络****接口**与**虚拟网络接口**间的通信。从功能上来看，Linux bridge 像一台虚拟交换机，所有桥接设置的虚拟机分别连接到这个交换机的一个接口上，接口之间可以相互访问且互不干扰，这种连接方式对物理主机而言也是如此。

![linuex bridge示意图](/Users/swk/studyTmp/linuex bridge示意图.png)



在桥接的作用下，虚拟网桥会把主机网络接口接收到的网络流量转发给虚拟网络接口，于是后者能够接收到路由器发出的 DHCP（动态主机设定协议，用于获取局域网 IP）信息及路由更新。这样的工作流程，同样适用于不同虚拟网络接口间的通信。具体的实现方式如下所示：

**虚拟机与宿主机通信**： 用户可以手动为虚拟机配置IP 地址、子网掩码，该 IP 需要和宿主机 IP 处于同一网段，这样虚拟机才能和宿主机进行通信。

**虚拟机与外界通信**： 如果虚拟机需要联网，还需为它手动配置网关，该网关也要和宿主机网关保持一致。

除此之外，还有一种较为简单的方法，那就是虚拟机通过 DHCP 自动获取 IP，实现与宿主机或宿主机以外的世界通信，小白亲测有效。





### Docker bridge 模式

大致清楚 Linux bridge 模式后，再来看 Docker bridge 模式，小白也有了信心。再次翻开《 Docker 进阶与实战》，仔细阅读后小白了解到在该 bridge 模式下，Docker Daemon 会创建出一个名为 docker0 的**虚拟网桥** ，用来连接**宿主机**与**容器**，或者连接**不同的容器**，书中的介绍与小白之前的假设也不谋而合。

Docker 利用 veth pair技术，在宿主机上创建了两个虚拟网络接口 veth0 和 veth1（veth pair 技术的特性可以保证无论哪一个 veth 接收到网络报文，都会无条件地传输给另一方）。

![docker bridge示意图](/Users/swk/studyTmp/docker bridge示意图.png)



**容器与宿主机通信** : 在桥接模式下，Docker Daemon 将 veth0 附加到 docker0 网桥上，保证宿主机的报文有能力发往 veth0。再将 veth1 添加到 Docker 容器所属的网络命名空间[注释2]，保证宿主机的网络报文若发往 veth0 可以立即被 veth1 收到。

**容器与外界通信** : 容器如果需要联网，则需要采用 NAT [注释2] 方式。准确的说，是 NATP (网络地址端口转换) 方式。NATP 包含两种转换方式：SNAT 和 DNAT 。

- 目的 NAT (Destination NAT，DNAT): 修改数据包的目的地址。



### 外界访问容器

当宿主机以外的世界需要访问容器时，数据包的流向如下图所示：

![dockerToInternet](/Users/swk/studyTmp/dockerToInternet.png)



由于容器的 IP 与端口对外都是不可见的，所以数据包的目的地址为**宿主机**的 **ip** 和**端口**，为 192.168.1.10:24 。

​		数据包经过路由器发给宿主机 eth0，再经 eth0 转发给 docker0 网桥。由于存在 DNAT 规则，会将数据包的目的地址转换为**容器**的 **ip** 和**端口**，为 172.17.0.n:24 。

​		宿主机上的 docker0 网桥识别到容器 ip 和端口，于是将数据包发送附加到 docker0 网桥上的 veth0 接口，veth0 接口再将数据包发送给容器内部的 veth1 接口，容器接收数据包并作出响应。

![dockerToInternetIp](/Users/swk/studyTmp/dockerToInternetIp.png)

- 源 NAT (Source NAT，SNAT): 修改数据包的源地址。



### 容器访问外界

当容器需要访问宿主机以外的世界时，数据包的流向为下图所示：

![internetToDocker](/Users/swk/studyTmp/internetToDocker.png)



此时数据包的源地址为**容器**的 **ip** 和**端口**，为 172.17.0.n:24，容器内部的 veth1 接口将数据包发往 veth0 接口，到达 docker0 网桥。

​		宿主机上的 docker0 网桥发现数据包的目的地址为外界的 IP 和端口，便会将数据包转发给 eth0 ，并从 eth0 发出去。由于存在 SNAT 规则，会将数据包的源地址转换为**宿主机**的 **ip** 和**端口**，为 192.168.1.10:24 。

​		由于路由器可以识别到宿主机的 ip 地址，所以再将数据包转发给外界，外界接受数据包并作出响应。这时候，在外界看来，这个数据包就是从 192.168.1.10:24 上发出来的，Docker 容器对外是不可见的。

![internetToDockerIp](/Users/swk/studyTmp/internetToDockerIp.png)



鸣谢：

http://blog.daocloud.io/docker-bridge/





[总结 - 自我思考]

三层网络关系

第一层：<qusetion>docker0如何进行port监听，找到需要进行NAT转换的报文的？ </qusetion>

<div style="color:red">iptable中配置转发规则</div>

第二层：<qusetion>docker0网桥，如何将port对应到veth0 键值对上的呢？ </qusetion>

<div style="color:red">内部有DNS进行转发解析</div>

第三层：veth - pair技术，将之间的网络报文在 `虚拟网桥` 和 `容器` 之间进行传输。



鸣谢：

http://qinghua.github.io/docker-bridge-network/

https://www.cnblogs.com/sanduzxcvbnm/p/13370773.html



----------

20210911更新

> Public Networking

> Let’s talk about how to publish a container port and IP addresses to the outside world. When you start a container using the `docker run` command, none of its ports are exposed. Your Docker container can connect to the outside world, but the outside world cannot connect to the container. To make the ports accessible for external use or with other containers not on the same network, you will have to use the `-P` (publish all available ports) or `-p` (publish specific ports) flag.



[Understanding Docker Networking]: https://earthly.dev/blog/docker-networking/#public-networking	"Public Networking"



### MAC中的docker 区别

* Mac OS 中的限制

　　没有 docker0 bridge 网卡。

　　宿主机不能 ping 通容器的ip，宿主机

　　不能从主机 Mac 访问 Linux bridge.

* mac 宿主机 和 容器互通 的解决方案

　　1、容器内访问宿主机，在 Docker 18.03 过后推荐使用 特殊的 DNS 记录 host.docker.internal 访问宿主机。但是注意，这个只是在 Docker Desktop for Mac 中作为开发时有效。 网关的 DNS 记录: gateway.docker.internal。原文 [Docker for Mac](https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds)

　　2、宿主机访问容器，使用本机 localhost 端口映射功能，使用 –publish（单个端口）, -p（单个端口）, -P（所有端口） 将本机的端口和容器的端口映射。

　　3、宿主机访问容器，使用 -p 参数映射端口。容器访问宿主机，可以在宿主机使用下面的命令获取 宿主机的 ip 地址：

```shell
ps -ef | grep -i docker | grep -i  "\-\-host\-ip" |awk -F "host-ip" '{print $2}' | awk -F '--lowest-ip' '{print $1}'
```

　　　　在宿主机的 mac 上查看 docker deamon的进程，可以找到启动的配置参数如下

```shell
ps -ef |grep -i docker 
```



鸣谢：

[csdn]: https://www.cnblogs.com/bjlhx/p/12111509.html	"001-docker-net-网络设置分类、Bridge详解、mac docker说明"

