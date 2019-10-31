Glacier是一项低成本的存储服务，可为数据存档和在线备份提供持久，安全且灵活的存储。 为了降低成本，Amazon Glacier专为不经常访问的数据而设计。

Glacier可以以任何格式存储无限量的几乎任何类型的数据。 常用场景包括替换传统磁带解决方案，以便进行长期备份以及归档存储合规性所需的数据。 在大多数情况下，存储在Glacier中的数据包含大型TAR或ZIP文件。

与S3一样，Glacier非常耐用（99.999999999％），可将数据存储在区域内多个设施的多个设备上。



# Archives

Glacier的数据存档最多可包含40TB的数据，而您可以拥有无限数量的存档。 每个存档在创建时都会分配一个唯一的存档ID。 （与S3对象Key不同，您无法指定存档名称。）所有存档都会自动加密，且不可变。



# Vaults

Vaults是存档的容器。每个AWS账号最多可拥有10,000个Vault。您可以使用IAM策略或者Vault的访问策略来控制对Vault的访问。



# Vault Lock

Vault锁定策略可帮助用户轻松实现Glacier数据合规性控制要求。例如，可以设置限制写入一次和多次读取策略，并锁定该策略，来控制数据的生成。还可设置读取限制，控制读取产生的费用在预期之内。




