# Kinesis

Kinesis是一个用于在AWS上处理大量流数据的平台，它提供了强大的服务，可以轻松加载和分析流数据，还可以让您构建满足特定需求的自定义流数据应用程序。Kinesis提供了三种服务满足不同的实时流数据需求：

• Kinesis Firehose：使您可以将大量流数据加载到AWS中

• Kinesis Streams：使您能够构建自定义应用程序，以实时更复杂地分析流数据

• Kinesis Analytics：使您可以使用标准SQL轻松轻松地实时分析流数据

Kinesis Firehose接收流数据并将其存储在S3，Redshift或Elasticsearch中。 您不需要编写任何代码。 只需创建一个数据传递流并配置数据流向的目标即可。 客户端使用AWS API调用将数据写入流中，并且数据会自动发送到正确的目的地。

当配置为将流保存到Amazon S3时，Kinesis Firehose将数据直接发送到S3。 对于Redshift目标，首先将数据写入S3，然后执行Redshift COPY命令以将数据加载到Redshift。 Kinesis Firehose还可以将数据写出到Elasticsearch，并可以选择将数据同时备份到S3。

对于任何巨大的数据来源，Amazon Kinesis Firehose都是确保所有数据成功存储在AWS架构中的绝佳选择。

Kinesis Streams使您可以实时收集和处理大型数据记录流。 您可以使用AWS开发工具包创建Kinesis Streams应用程序，该应用程序在数据流动时对其进行处理。 因为数据获取和处理的时间几乎是实时的，所以处理通常是轻量级的。 Kinesis Streams可以通过跨多个分片分配传入数据来扩展以支持几乎无限的数据流。 如果其中的某些分片负载过大，可以将其进一步划分为更多的分片以进一步分配负载。 然后在使用者上执行该处理，使用者从分片中读取数据并运行Kinesis Streams应用程序。

CloudFormation 为您提供了一种通用语言来描述和预置您的云环境中的所有基础设施资源。CloudFormation 使您可以跨所有地区和账户，使用编程语言或简单的文本文件以自动化的安全方式，为您的应用程序需要的所有资源建模并进行预置。这为您提供了 AWS 资源的单一数据源。您无需为 AWS CloudFormation 支付额外费用，只需支付支持您应用程序的运行的 AWS 资源费用。



