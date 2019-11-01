CloudWatch是用于实时监控您的云资源及应用程序的服务。借助CloudWatch您可以根据定义的规则收集和跟踪指标，发送警报及根据监控对资源进行调整。

例如，您可能选择监视CPU占用率，以确定何时应该在应用程序层中添加或删除EC2实例。 或者，如果某个AWS上看不见的特定指标是评估扩展需求的最佳指标，可以执行PUT请求将该指标推送到Amazon CloudWatch。 然后，您可以使用此自定义指标来管理能力需求。

您还可以在一段时间内为指标指定参数，并在达到阈值时配置警报和自动操作。 Amazon CloudWatch支持多种类型的操作，例如将通知发送到Amazon Simple Notification Service（Amazon SNS）主题或执行Auto Scaling策略。

CloudWatch为受支持的AWS产品提供基本或高级监视。 基本监控每五分钟将数据发送到CloudWatch，以免费的方式提供有限数量的预选指标。 高级监控会每分钟将数据发送到CloudWatch，并允许数据聚合分析，需要额外付费。 CloudWatch的默认设置是基本监控。

CloudWatch支持大多数AWS Cloud服务的监视和特定指标，包括：Auto Scaling，Amazon CloudFront，Amazon CloudSearch，Amazon DynamoDB，Amazon EC2，Amazon EC2容器服务（Amazon ECS），Amazon ElastiCache，Amazon Elastic Block Store（Amazon EBS） ，弹性负载平衡，Amazon Elastic MapReduce（Amazon EMR），Amazon Elasticsearch Service，Amazon Kinesis Streams，Amazon Kinesis Firehose，AWS Lambda，Amazon Machine Learning，AWS OpsWorks，Amazon Redshift，Amazon Relational Database Service（Amazon RDS），Amazon Route 53 ，Amazon SNS，Amazon Simple Queue Service（Amazon SQS），Amazon S3，AWS Simple Workflow Service（Amazon SWF），AWS Storage Gateway，AWS WAF和Amazon WorkSpaces。

您可以通过执行GET请求来检索CloudWatch指标。 使用高级监控还可以在指定的时间范围内聚合指标。  CloudWatch不支持跨区域聚合数据，但可以跨区域内的聚合可用区。

AWS为每个服务提供了一组丰富的指标，您也可以自定义指标以监视AWS无法查看的资源和事件。CloudWatch支持应用程序编程接口（API），该应用程序编程接口（API）允许程序和脚本以名称-值对的形式将指标放入Amazon CloudWatch，然后可用于创建事件并以与默认Amazon CloudWatch指标相同的方式触发警报。

CloudWatch Logs可用于监视，存储和访问来自Amazon EC2实例，AWS CloudTrail和其他来源的日志文件。 然后，您可以检索日志数据并实时监视事件-例如，您可以跟踪应用程序日志中的错误数，并在错误率超过阈值时发送通知。 Amazon CloudWatch Logs也可以将您的日志存储在Amazon S3或Amazon Glacier中。 日志可以无限期保留，也可以根据策略保留，该策略将删除不再需要的旧日志。

CloudWatch Logs代理提供了一种方式自动将日志数据发送到运行Linux或Ubuntu的EC2实例的CloudWatch Logs。 您可以在现有的EC2实例上使用CloudWatch Logs代理安装程序来安装和配置代理。 安装完成后，代理会启动并保持运行状态，直到您禁用为止。

使用CloudWatch服务时应记住有一些限制。譬如每个AWS帐户每个AWS帐户限制为5,000个警报，默认情况下（在本书撰写时），指标数据将保留两周。 如果您想保留更长的数据，则需要将日志移至S3或Glacier之类的持久性存储中。



