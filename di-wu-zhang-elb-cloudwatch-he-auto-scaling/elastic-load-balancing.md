拥有大量云上服务器，如果能向用户提供一致性服务，将是无可比拟的优势。确保一致性的一种方法就是在多台服务器之间实现负载均衡。负载均衡是一个在EC2实例间分配流量的机制。 您可以在Amazon EC2实例上管理自己的虚拟负载均衡器，也可以利用称为Elastic Load Balancing的AWS云服务，该服务为您提供托管负载均衡器。

ELB服务在一个或多个可用区的EC2实例间分配流量，使应用程序实现高可用性。ELB支持通过HTTP、HTTPS、TCP和SSL协议传输的流量负载均衡。ELB为DNS配置提供了稳定的单一规范名称记录（CNAME）入口点，并且支持面向Internet和内部应用程序的负载均衡。ELB还支持对EC2实例的运行状况进行检查，以确保流量不会被路由到不可用节点。 此外，Elastic Load Balancing可以根据收集的指标实行自动扩展。

使用ELB有几个优点。 由于ELB是一项托管服务，因此它会自动扩展以满足不断增长的应用程序流量的需求，并且在区域本身即服务中具有很高的可用性。 ELB通过在多个可用区中的正常实例之间分配流量来帮助您实现应用程序的高可用性。 此外，ELB与Auto Scaling服务无缝集成，配合实现实例自动调整。 最后，Elastic Load Balancing是安全的，可与VPC一起在应用程序层之间转发流量，从而仅需暴露面向Internet的公共IP地址，隐藏其他所有信息。 Elastic Load Balancing还支持集成证书管理和SSL停用。



# Types of Load Balancers

ELB提供了几种类型的负载平衡用于处理不同类型的连接，包括支持加密的面向Internet和内部的负载平衡器。

## 面向Internet

顾名思义，面向Internet的负载均衡通过Internet接收来自客户端的请求，并将其分配给在负载均衡器中注册的EC2实例。

配置负载均衡时，ELB需要一个公共DNS，让客户端可以使用该DNS将请求发送到您的应用程序。 DNS服务器将该DNS解析为您的负载均衡器的公共IP地址，客户端应用程序可以访问该IP地址。

由于ELB可以进行扩展以满足流量需求，因此不建议将应用程序绑定到IP地址，该IP地址可能不属于负载均衡器资源池。

 VPC中的ELB仅支持IPv4地址。 EC2-Classic中的弹性负载平衡则同时支持IPv4和IPv6地址。

## 内部负载均衡

在多层结构的应用程序体系中，ELB在应用程序层显得非常有用。例如，面向Internet的负载均衡接收到外部请求并将其分配到界面层或者web层，然后将其转发到业务层之前的ELB。

## HTTPS负载均衡

您可以创建支持SSL /TLS协议用于加密连接（也称为SSL）的负载平衡器。 此功能启用了负载均衡器和启动HTTPS会话对客户端之间的通信加密，以及负载均衡器和后端实例之间连接的流量加密。 ELB提供已预定义SSL配置的安全策略供使用。 为了使用SSL，您必须在用于截断连接和加密信息的负载均衡器上安装SSL证书，然后在将请求发送到后端EC2实例之前解密来自客户端的请求。

ELB不支持SNI。这意味着如果您想使用只安装了一个SSL证书的负载均衡后边解析多个网站，需要为该证书添加每个网站的SAN，以避免网站用户在访问网站时弹出警告信息。

# Listeners

每个负载平衡器必须配置一个或多个侦听器（Listener）。 侦听器是检查连接请求的处理过程，例如，将CNAME配置为负载均衡器的主机记录（A记录）。 每个侦听器都配置有用于前端连接的协议和端口（客户端到负载均衡器）以及用于后端（负载均衡器到实例）连接的协议和端口。 ELB支持HTTP,HTTPS,TCL和SSL协议。

# 配置ELB

ELB提供了多方面的设置以灵活使用。包括 idle connection timeout, cross-zone load balancing, connection draining, proxy protocol, sticky sessions, 和health checks。可使用AWS管理控制台或者CLI来修改配置。

## Idle Connection Timeout

对于客户端通过负载均衡器发出的每个请求，负载均衡器都会维护两个连接。 一个是与客户端的连接，另一个是与后端实例的连接。 对于每个连接，负载均衡器管理一个空闲时限，如果在指定时间段内未通过该连接发送任何数据时，空闲时限会被触发，负载均衡器将关闭连接。

默认情况下，ELB将两个连接的空闲时限设置为60秒。如果请求在60秒内未完成数据传输，尽管数据还在传输中，ELB也将关闭连接。默认空闲时限可以修改为其他时间，已支持大数据量传输。

如果您使用HTTP和HTTPS侦听器，我们建议您为EC2实例启用keep-alive选项。启用keep-alive后，负载平衡器可以重新使用与您的后端实例的连接，从而降低CPU占用率。

## Cross-zone LB

为保证请求流量在所有后端上保持均衡，不管可用区在哪，都应启用Cross-zone 负载均衡。Cross-zone负载均衡减少了在每个可用区中维护相等数量的后端实例的需求，并提高了您的应用程序处理一个或多个后端实例丢失的能力。 但是，仍然建议您在每个可用区中维护大约相当数量的实例，以提高容错能力。

对于在客户端缓存DNS的场景下，请求可能会倾向于某个可用区域而导致失衡。使用Cross-zone负载均衡，可以将倾向性负载平衡在所有后端上，从而减小因客户端错误配置而造成的影响。

## Connection Draining

启用connection Draining选项，可以防止负载均衡器将请求分配到正在注销或者运行不正常的实例上，同时，保持现有连接处于打开状态，确保完成任务。

启用Connection Draining后，可以为负载均衡制定最大时间，在实例被注销之前，让连接保持活动状态。时间可以设置在1到3600秒之间，默认是300秒。当最大时间到达之后，负载均衡器强行关闭实例连接。

## Proxy Protocol

当您将TCP或SSL用于前端和后端连接时，负载平衡器会将请求转发到后端实例，而无需修改请求的头部信息。如果启用了Proxy Protocol选项，可读的信息会被加入到请求头部信息之中，并附带连接信息，如源IP地址，目标IP等，并将该信息作为请求的一部分发送到后端。

在启用Proxy Protocol之前，请确认负载均衡没有部署在启用了代理协议的服务器之后。如果服务器和负载均衡都启用了Proxy Protocol，则请求可能会被添加两个表头，从而导致错误。

## Sticky Sessions

默认情况下，负载均衡会将每个请求独立的分配到负载低的实例上。启用Sticky Sessions选项，可以将session与特定实例绑定，这样可以让某个session期间的所有请求发送到同一个实例。

管理sticky sessions的关键是如何决定负载均衡特定管理用户请求的时间。如果应用程序具备自己的cookie，可以配置时间为cookie的持续时间，如果没有cookie，则可以让ELB创建cookie来制定持续时间。ELB会创建一个名为AWSELB的cookie，将session映射到实例。

# Health Check

ELB支持health check以对服务器运行状态进行检查。inService表示运行状况良好的服务器实例，运行异常的服务器会被标为OutOfService。

ELB通过ping命令对所有注册过的实例进行检查。您可以设置检查的时间间隔，还可以设置检查的响应时间，最后，您还可以设置一个检查失败的次数为阈值，来决定是否给实例亮红灯。


