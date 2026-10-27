# REST是什么？

REST是表述性状态传递，英文全称是Representational State Transfer，需要注意的是，REST是设计风格而不是标准。REST的架构风格是针对Web应用设计和开发的，它使得HTTP协议的原本面貌得以被了解。使用REST可以降低开发的复杂性，提高系统的可伸缩性。如今，REST架构已经成为主流技术。

目前在三种主流的Web服务实现方案中，因为REST模式的Web服务与复杂的SOAP和XML-RPC对比来讲明显的更加简洁，越来越多的web服务开始采用REST风格设计和实现。例如，Amazon.com提供接近REST风格的Web服务进行图书查找；雅虎提供的Web服务也是REST风格的。

# REST的设计标准

------

1. 网络上的所有事物都被抽象为资源（resource）；
2. 每个资源对应一个唯一的资源标识符（resource identifier）；
3. 通过通用的连接器接口（generic connector interface）对资源进行操作；
4. 对资源的各种操作不会改变资源标识符；
5. 所有的操作都是无状态的（stateless）。