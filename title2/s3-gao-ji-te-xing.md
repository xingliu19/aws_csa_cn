# Prefixes and Delimiters

虽然S3在存储桶中的层级扁平，它仍可以支持使用前缀和分隔符。这能间接实现按层次结构来组织存储对象。譬如，您可以使用“/”或者“\”来做分隔符，嵌入到对象名中实现分层。譬如：

logs/2016/January/server42.log

logs/2016/February/server42.log



REST API、CLI、SDK等都支持使用前缀和分隔符。这能丰富您管理存储对象的手段方式。您还可以把前缀、分隔符与IAM结合应用，限制对某些“目录”的访问权。



# Storage Classes

S3提供了一系列的类供存储使用。

S3 Standard是设计为通用的高持久高可用，高性能的存储类，由于它具有较低的首字节延迟和高吞吐量，非常适合频繁访问数据的长短期存储。 对于大多数应用场景，都可以使用S3 Standard。

S3 Standard-IA提供与Standard相同的持久性和性能，专用于长期存储但读取频率低的数据。Standard-IA的GB/月成本低于S3 Standard，其定价策略还包括对象大小，最短持续时间和GB检索成本，因此适用于访问间隔超30天的数据存储。

S3 RSS以低成本提供比标准或标准IA略低的耐用性（4个9）。 它最适合于可以轻松复制的派生数据，例如图像缩略图。

最后，Glacier设计的目的是应用于非实时数据存储。为了降低成本，Glacier针对数据的读取进行了针对性设计，适合数据读取时间需要几小时的场景。

检索Glacier对象，首先需要使用S3API发出restore命令，等待（3-5小时）Glacier对象复制到S3 RSS。然后进行读取。Glacier提供每月免费检索5%的数据，超出部分收费。

Glacier除了担当S3的存储外，它还是一个独立的存储服务，具备自己的API和特性。如果您把Glacier当成S3的存储使用，你始终可以使用S3 API与数据进行互动。



# Object Lifecycle Management

S3对象生命周期管理大致相当于传统IT存储架构中的自动分层。 在许多情况下，数据具有自然生命周期，从“热”（频繁访问）数据开始，随着年龄的增长转向“温暖”（不常访问）数据，到“冷”（长期备份）数据状态 或最终删除。

例如，许多业务数据在创建初期会被经常访问，随着时间推移，访问频率越来越低。但是合规要求业务数据是需要保持数年时间的。研究表明，操作系统和数据库在被回复后的几天内，访问的频率也较高，在其后的一两周内，仍是重要的资产，但再恢复的可能性就低了很多。因此，合规性要求数据的保存期限要达到数年。

使用S3生命周期配置规则，您可以通过自动将数据从一个存储类转换为另一个存储类或在一段时间后自动删除数据来降低存储成本。 例如，给数据设置如下备份规则：

	• 最初将备份数据存储在S3 Standard中。

	• 30天后，过渡到Standard-IA。

	• 90天后，过渡到Glacier。

	• 3年后，删除。

生命周期配置可以应用于存储桶中的所有对象，也可以仅应用于制定前缀对象。



# Encryption

强烈建议对存储在Amazon S3中的所有敏感数据进行加密，包括在使用中和静止时。

要在使用时加密S3数据，您可以使用S3 SSL API。 这可确保在传输过程中对发送到S3和从S3发送的所有数据使用HTTPS协议进行加密。

加密 S3 静态数据，您可以使用SSE。 Amazon S3在对象级别对数据进行加密，它将数据写入数据中心的磁盘，并在访问时为您解密。 Amazon S3和AWS Key Management Service（Amazon KMS）执行的所有SSE均使用256位高级加密标准（AES）。 



# Versioning

S3的版本控制机制通过在存储桶中保留每个对象的多个版本（由唯一版本ID标识）来帮助保护您的数据免遭意外或恶意删除。

通过版本控制，您可以保留，检索和还原存储在Amazon S3存储桶中的所有对象的所有版本。 如果用户有意或无意删除S3存储桶中的对象，则只需引用存储桶和对象键之外的版本ID，即可将对象还原到其原始状态。 版本控制基于存储桶级别，启用后，您江无法从存储桶中删除版本控制; 它只能被暂停。



# MFA Delete

MFA Delete在存储桶版本控制之上添加了另一层数据保护。 除了正常的安全凭证之外，MFA删除还需要由硬件或虚拟多重身份验证（MFA）设备生成的身份验证代码（临时一次性密码）。 请注意，MFA删除只能由root帐户启用。



# Pre-Signed URLs

默认情况下，所有S3对象都是私有的，这意味着只有所有者才能访问。

对象所有者可以通过创建预签名URL与其他人共享对象，他人可以使用自己的安全凭证在授权期限内下载对象。 为对象创建预签名URL时，必须提供安全凭据并指定存储桶名称，对象Key，HTTP方法（GET）以及到期日期和时间。 预签名URL仅在指定的时间内有效。





