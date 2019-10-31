本章将学习Elastic Load Balancing，Amazon CloudWatch和Auto Scaling如何独立且一起工作，实现高效，经济地部署高可用性和优化的架构。

Elastic Load Balancing是一项高可用性服务，可在Amazon Elastic Compute Cloud（Amazon EC2）实例之间分配流量，并提供一些灵活的选项控制对Amazon EC2实例的传入请求。

Amazon CloudWatch是一项用于监视AWS上运行的AWS Cloud资源和应用程序的服务。它收集并跟踪指标，收集和监视日志文件，并设置警报。 Amazon CloudWatch提供免费的基础级别的监控服务，而更高级别的监控服务则需要额外的费用。

Auto Scaling是一项可让您根据设置的条件通过向上或向下扩展Amazon EC2容量来维护应用程序的可用性的服务。

本章分别介绍了这三种服务，但同时也着重介绍了它们如何协同工作以在AWS上构建更健壮和高度可用的架构。

