# RPC框架

>   参考文档：[RPC框架](https://snailclimb.gitee.io/javaguide/#/docs/system-design/data-communication/why-use-rpc)

## RPC简介

**什么是 RPC？**

RPC（Remote Procedure Call）—远程过程调用，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。比如两个不同的服务 A、B 部署在两台不同的机器上，那么服务 A 如果想要调用服务 B 中的某个方法该怎么办呢？使用 HTTP请求当然可以，但是可能会比较慢而且一些优化做的并不好。 RPC 的出现就是为了解决这个问题。

## RPC原理

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4olrump8j30dv08t77k.jpg" alt="RPC原理图" style="zoom:67%;" />

1.  服务消费方（client）调用以本地调用方式调用服务；
2.  client stub接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体；
3.  client stub找到服务地址，并将消息发送到服务端；
4.  server stub收到消息后进行解码；
5.  server stub根据解码结果调用本地的服务；
6.  本地服务执行并将结果返回给server stub；
7.  server stub将返回结果打包成消息并发送至消费方；
8.  client stub接收到消息，并进行解码；
9.  服务消费方得到最终结果。

时序图：

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4ondbhbfj30gh0fsq51.jpg" alt="RPC原理时序图"  />

## RPC 解决了什么问题？

**让分布式或者微服务系统中不同服务之间的调用像本地调用一样简单。**

## 常见的 RPC 框架

*   **RMI（JDK自带）：** JDK自带的RPC，有很多局限性，不推荐使用。
*   **Dubbo:** Dubbo是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和 Spring框架无缝集成。目前 Dubbo 已经成为 Spring Cloud Alibaba 中的官方组件。
*   **gRPC** ：gRPC是可以在任何环境中运行的现代开源高性能RPC框架。它可以通过可插拔的支持来有效地连接数据中心内和跨数据中心的服务，以实现负载平衡，跟踪，运行状况检查和身份验证。它也适用于分布式计算的最后一英里，以将设备，移动应用程序和浏览器连接到后端服务。
*   **Hessian：** Hessian是一个轻量级的remotingonhttp工具，使用简单的方法提供了RMI的功能。 相比WebService，Hessian更简单、快捷。采用的是二进制RPC协议，因为采用的是二进制协议，所以它很适合于发送二进制数据。
*   **Thrift：** Apache Thrift是Facebook开源的跨语言的RPC通信框架，目前已经捐献给Apache基金会管理，由于其跨语言特性和出色的性能，在很多互联网公司得到应用，有能力的公司甚至会基于thrift研发一套分布式服务框架，增加诸如服务注册、服务发现等功能。

## HTTP和RPC比较

RPC 是一种概念、一种设计，就是为了解决 **不同服务之间的调用问题**, 它一般会包含有 **传输协议** 和 **序列化协议** 这两个。

HTTP 是一种协议，RPC框架可以使用 HTTP协议作为传输协议或者直接使用TCP作为传输协议，使用不同的协议一般也是为了适应不同的场景。

