AWS云的一个优势在于，它允许客户在安全环境下进行扩展和创新。 云安全性与本地数据中心安全性没有差异，而且没有维护设施和硬件的成本。 在云中，您不必管理物理服务器或存储设备，仅需使用基于软件的安全性工具来监视和保护进出云资源的信息流。



# AWS Directory Service



AWS Directory Service是一项托管服务产品，其提供的目录包含有关您的组织的信息，包括用户，用户组，计算机和其他资源。您可以选择的目录有三种类型： 企业版AD，简单版AD和AD Connector。



企业版AD 适用于微软企业版Active Directory，它提供了Microsoft Active Directory提供的许多功能以及与AWS应用程序的集成。 借助其他Active Directory功能，您可以例如轻松地与现有Active Directory域建立信任关系，以将这些目录扩展到AWS云服务。如果您拥有超过5,000个用户，并且需要在AWS托管的目录与本地目录之间建立信任关系，它将是您的最佳选择。

简单AD Simple AD是与微软AD兼容的目录服务，有Samba 4提供支持。SimpleAD支持常用的Active Directory功能，例如用户帐户，组成员身份，运行Linux和Microsoft Windows的加入域的Amazon EC2实例，Kerberos 基于的单一登录（SSO）和组策略。 这使管理运行Linux和Windows的Amazon EC2实例以及在AWS云上部署Windows应用程序变得更加容易。

MS AD支持的应用程序和工具都可以与Simple AD一起使用，Simple AD中的用户也可以访问AWS程序，如WorkSpace，WorkDoc等。还可以使用IAM角色管理AWS资源，Simple AD还提供每日自动快照，以便随时恢复。

Simple AD 是最便宜的选择，如果您拥有5,000个或更少的用户并且不需要更高级的Microsoft Active Directory功能，它将是最佳选择。

AD Connect 用于连接本地AD代理服务和AWS，没有目录同步的复杂性，或者托管基础结构的高成本。AD连接器将登录请求转发到Active Directory域控制器以进行身份验证，并使应用程序能够查询目录中的数据。设置后，您的可以使用现有的公司凭证登录到AWS应用程序，例如WorkSpaces，WorkDocs或WorkMail。 拥有适当的IAM权限还可以访问AWS管理控制台并管理AWS资源。当您要使用现有的内部部署时，AD连接器是您访问AWS云服务的目录的最佳选择。



