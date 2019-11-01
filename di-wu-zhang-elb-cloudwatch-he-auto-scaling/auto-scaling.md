将应用程序部署到云的一个明显优势是能够响应可变的工作负载启动和释放服务器。 按需配置服务器，然后在不再需要它们时将其释放从而节省大量成本。

Auto Scaling是一项可让您根据定义的条件横向扩展和纵向扩展EC2能力的服务。 借助Auto Scaling，您可以确保正在运行的Amazon EC2实例的数量在需求高峰期间增加，以维持应用程序性能，并在需求停顿或低谷期间自动减少，以最大程度地降低成本。

# Auto Scaling Plans

Auto Scaling具有几种方案或计划，可用于控制Auto Scaling的执行方式。

## Maintain Current Instance Levels

您可以配置Auto Scaling组以始终保持最少或指定数量运行实例。 为了维持当前实例水平，Auto Scaling会对Auto Scaling组中正在运行的实例执行定期运行状况检查。 当Auto Scaling找到不正常的实例时，它将终止该实例并启动一个新实例。

## Manual Scaling

手动是扩展资源的最基本方法。 您只需要指定Auto Scaling组的最大，最小或所需容量即可。 Auto Scaling会自动创建或终止实例的过程以维护资源。

## Scheduled Scaling

当您确切知道性能需求的变化周期，您可以根据时间日期来设定缩放比例来对EC2实例进行收缩或扩展。

## Dynamic Scaling

您还可以定义控制策略中的自动缩放过程的参数。 例如，您可以创建一个策略，当由CloudWatch监视的网络带宽达到一定阈值时，将更多Amazon EC2实例添加到Web层。



# Auto Scaling Components

Auto Scaling需要几项配置后才能正常工作：启动配置、Auto Scaling组和缩放策略（可选）。

## 启动配置

启动配置是Auto Scaling用来创建实例时用的模板，它由名称，AMI，实例类型，安全组和秘钥对组成，每个Auto Scaling组只能有一个启动配置。

Auto Scaling可能会让您使用云资源时达到服务的限制，因此在使用AWS构建复杂架构时，请务必牢记该服务在AWS Cloud Services上的限制。

Auto Scaling Group

Auto Scaling组是由Auto Scaling管理的EC2实例集合。 每个Auto Scaling组都包含配置选项，这些选项控制Auto Scaling应何时启动或终止实例。 Auto Scaling组必须包含组名以及组中可以包含的实例最小和最大数目。 您也可以选择指定所需的容量，让组始终保持一个相应的实例数目。 如果您未指定容量，则默认的所需容量是配置的最小实例数。

## Scaling Policy

您可以将CloudWatch警报和扩展策略与Auto Scaling组关联以动态调整资源。 超过阈值时，Amazon CloudWatch会发送警报以触发对Amazon EC2实例数量的更改（放大或缩小）来应对经过负载均衡器的网络流量。 在CloudWatch警报将消息发送到Auto Scaling组后，Auto Scaling将执行关联的策略来扩展您的组。 该策略是一组指令，告诉Auto Scaling是根据启动配置中实例策略来启动实例扩展资源，还是终止实例释放资源。

实例的缩放可以是制定实例数量，或者百分比。

在一个Auto Scaling组里可以有多个策略。例如，您可以根据CPU占用率触发器（CPULoad）和CloudWatch测量器（CPUUtilization）来创建策略，在制定2分钟内CPU占用率大于75%的时候扩充资源。您可以将一个策略附加到同一个组，在20分钟内CPU占用率不足40%的时候。

推荐最佳实践是快速扩展并缓慢释放，以便您可以对突发或峰值做出响应，同时避免意外终止Amazon EC2实例，如果性能需求持续爆发，则仅必须启动更多Amazon EC2实例。 Auto Scaling还支持冷却时间，该时间是可配置的设置，它确定何时暂停Auto Scaling组的扩展活动。



