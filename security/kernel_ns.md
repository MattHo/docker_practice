##内核名字空间
Docker容器和LXC容器很相似，他们提供的安全特性也差不多。当你用`docker run`启动一个容器时，在后台Docker为容器创建了一个名字空间和控制组的集合。

名字空间提供了最初也是最直接的隔离，在容器中运行的进程不会被运行在主机上的进程和容器发现，他们之间相互影响也就小了。

每个容器都有自己的网络堆栈，他们不能访问其他容器的sockets接口。不过，如果在主机系统上做了相应的设置，他们还是可以像跟主机交互一样的和其他容器交互通信。当你指定公共端口或则使用links来连接2个容器时，他们就可以相互通信了。（相互ping、udp、tcp都没问题，也可以根据需要设定更严格的策略）从网络架构上来看，所有的容器通过主机的网桥接口相互通信，就像物理机器通过物理交换机通信一样。

内核提供的名字空间和私有网络的代码有多成熟？

内核名字空间从内核2.6.15之后被引入，距今已经5年了，在很多大型生产系统中被验证。他们的设计和灵感提出的时间更早，openvz项目利用名字空间重新封装他们的内核，并合并到主流内核中。openvz最早的版本在2005，所以其设计和实现都很成熟。