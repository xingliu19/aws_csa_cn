SQS是一项快速、可靠、可扩展的完全托管消息队列服务。SQS使云上业务应用的解耦变得简单高效。您可以使用SQS处理任何体量数据，而不需要担心数据的丢失。

SQS可以帮您降低操作和管理消息集群的工作复旦，并且仅需低廉的费用。使用SQS，您可以将应用程序消息存储在可靠且可扩展的基础架构上，消息可以在分布式组件间移动，执行不同的任务。

本质上来说，SQS队列是接收数据的组件和处理数据的组件之间的缓冲区。如果处理数据的应用程序难以及时处理传递的消息，消息会保存在队列中，服务器准备就绪后，可从队列中取出等候的消息进行处理，这意味着用户不需要担心因为资源的问题而遗失工作。

 SQS确保每个消息至少传递一次，并支持与同队列交互的多个读取器和写入器。 多个分布式应用程序组件可以同时使用一个队列，而无需组件之间互相协调。 大多数情况下，每条消息都会一次性准确地传递给应用程序，消息重复传递给应用的现象是可能存在的，所以应用程序的设计要避开这种情况。

SQS服务不能保证消息的先进先出（FIFO）传递。 对于分布式应用程序来说，每个消息可以独立存在，顺序并不重要。 如果系统对消息顺序有需求，应该在每个消息中放置顺序信息，以便在从队列中检索消息时可以对消息进行排序。



# 消息生命周期

消息在队列中的处理顺序及其生命周期在以下步骤中体现：

1. 组件1将消息A发送到队列

2. 当组件2准备就绪，从队列中检索得到消息A。 在处理消息A时，消息A仍保留在队列中，在其可见周期内不会再被检索到。

3. 组件2处理完毕后，从队列中删除消息A，以防止可见周期后再次接收和处理该消息。



# 延迟队列和可见周期

延迟队列可使消息传递延迟特定秒数。在延迟期间，消息处理者看不到发送到队列中的消息。使用延迟队列，请使用CreateQueue并将DelaySeconds设置为0到900秒的数值。还可以用SetQueueAttributes设置队列的DelaySeconds属性，将现有队列变成延迟队列。 DelaySeconds的默认值为0。

延迟队列与可见周期类似，这两个功能都使消息在特定时间段内对消费者不可用。 区别在于，延迟队列在第一次添加到队列时会隐藏消息，而可见周期仅在从队列中检索到消息后才隐藏消息。

当消息在队列中但既没有延迟也没有可见周期时，即被视为“正在运行”。在任意给定时间内，您最多可以有120,000条正在运行的消息。 Amazon SQS最多支持12个小时的最大可见周期。



# 队列操作、ID和消息元数据

SQS队列的已定义操作是CreateQueue，ListQueues，DeleteQueue，SendMessage，SendMessageBatch，ReceiveMessage，DeleteMessage，DeleteMessageBatch，PurgeQueue，ChangeMessageVisibility，ChangeMessageVisibilityBatch，SetQueueAttributes，GetQueueAttributes，GetQueueUrl，ListDeadLetterSourceQueues，AddPermission和RemovePermission。 只有AWS账户所有者或已被授予适当权限的AWS身份才能执行操作。

消息被推送入队列时，SQS会返回消息的全局ID。ID对于操作消息来说不是必要的，但对于跟踪消息是否被接收十分有用。当你从队列中接收一个消息，响应信息会包含一个句柄，在删除消息的时候，必须提供该句柄。



# 队列和消息标识符

对于SQS的应用，您需要熟悉的三个标识符：队列URL，消息ID和收据句柄。

创建新队列时，必须提供一个全局范围内唯一的队列名称。 SQS为每个队列分配一个称为队列URL的标识符，该标识符包括队列名称和SQS确定的其他组件。 对队列执行操作时，都必须提供其队列URL。

SQS为每个消息分配一个唯一的ID，该ID将在SendMessage响应中返回。 该标识符对于识别消息很有用，但是请注意，要删除消息，您需要提供消息的接收句柄而不是消息ID。 消息ID的最大长度为100个字符。

每次从队列收到消息时，都会收到该消息的接收句柄。 该句柄与接收消息的行为相关联，而不是与消息本身相关联。 如前所述，要删除消息或更改消息可见性，必须提供接收句柄而不是消息ID。 这意味着您必须收到一条消息，然后才能删除它。 接收句柄的最大长度为1,024个字符。



# 消息属性

消息属性允许您提供有关消息的结构化元数据项（例如时间戳，地理空间数据，签名和标识符）。它是可选的，与消息正文独立但同时发送出去。消费者可通过消息属性来决定如何处理消息。每个消息最多可以具有10个属性。 要指定消息属性，您可以使用AWS管理控制台，AWS软件开发套件（SDK）或API。



# 长轮询

函数ReceiveMessage用于查询在SQS队列中的消息。 ReceiveMessage检查队列中是否存在消息并立即返回，无论消息是否存在。 如果您的代码定期查询队列消息，则此模式就足够了。 但是，如果您的SQS客户端是一个反复检查新消息的循环，则此模式会出现问题，因为对ReceiveMessage的不断调用会消耗CPU并占用线程。

这种情况下，您需要使用长轮询。 使用长轮询时，可将最长20秒的WaitTimeSeconds参数发送到ReceiveMessage。 如果队列中没有消息，将等待WaitTimeSeconds，如果在时间到期之前出现消息，则立即返回该消息。 


