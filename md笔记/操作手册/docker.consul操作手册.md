windows11 下docker搭建consul单机和集群基础配置如下：
单机
docker获取consul并创建容器的步骤

1.   docker pull consul
2.   docker run --name consul -d -p 8500:8500 -p 8600:8600/udp consul

集群
 建立consul集群命令步骤

1.   建立第一个容器，并启动第一个consul服务

     docker run --name consul1 -d -p 8500:8500 -p 8300:8300 -p 8301:8301 -p 8302:8302 -p 8600:8600 consul agent -server -bootstrap-expect 2 -ui -bind=0.0.0.0 -client=0.0.0.0

     上诉命令字段解析

     1). 8500 http 端口，用于 http 接口和 web ui
     2). 8300 server rpc 端口，同一数据中心 consul server 之间通过该端口通信
     3). 8301 serf lan 端口，同一数据中心 consul client 通过该端口通信
     4). 8302 serf wan 端口，不同数据中心 consul server 通过该端口通信
     5). 8600 dns 端口，用于服务发现
     6). -bbostrap-expect 2: 集群至少两台服务器，才能选举集群leader
     7). -ui：运行 web 控制台
     8). -bind： 监听网口，0.0.0.0 表示所有网口，如果不指定默认未127.0.0.1，则无法和容器通信
     9). -client ： 限制某些网口可以访问

2.   获取第一个容器IP地址
       docker inspect --format "{{ .NetworkSettings.IPAddress }}" consul1
       \# 输出是：172.17.0.2

3.   启动第二个consul服务：consul2， 并加入consul1（使用join命令）
      docker run --name consul2 -d -p 8501:8500 consul agent -server -ui -bind=0.0.0.0 -client=0.0.0.0 -join 172.17.0.2

4.   启动第三个consul服务：consul3， 并加入consul1（使用join命令）
      docker run --name consul3 -d -p 8502:8500 consul agent -server -ui -bind=0.0.0.0 -client=0.0.0.0 -join 172.17.0.2
     \# .......(同样的步骤，可以启动第四，第五甚至更多的consul服务)
     \# 宿主机浏览器访问：http://localhost:8500 或者 http://localhost:8501 或者 http://localhost:8502



鸣谢：https://time.geekbang.org/course/detail/100023501-95651 知易老哥的评论

