IAM首先要理解的principal。 Principal是允许与AWS资源进行交互的IAM实体。 它可以是永久的或临时的，并且可以代表人员或应用程序。 Principal有三种类型：root用户，IAM用户和角色/临时安全令牌。



# ROOT用户

首次创建AWS账号时，您将以一个拥有完全访问权限的用户开始。这个就是root user，它会一直与您的AWS账号共同存在。root用户可用于控制台和对AWS资源的编程访问。

ROOT 用户与Unix的root和Windows的administrator类似，它有完全特权。因此，强烈不建议将root用户用于日常任务，包括管理任务。最佳实践是用root创建IAM用户，把root用户保护起来。



# IAM用户

IAM用户是通过IAM服务创建的用户，代表个人或者应用。 您可以为运营团队的每个成员创建单独的IAM用户，以便他们可以与控制台进行交互并使用CLI。您可能还需要为访问AWS Cloud服务的应用程序创建开发，测试和生产用户。

IAM用户可通过AWS管理控制台，CLI或SDK创建，由具有权限的具有IAM管理权的principal负责创建。它们创建后，一直存在直至被管理员删除。



# Roles/Temporary Security Tokens

角色/临时令牌对高级IAM使用很重要。角色用于特定时间段内给特定用户授予特定权限，前提是这些特定用户能通过AWS或者外部受信任系统的身份认证。当用户被加入角色时，AWS向用户提供来自STS的临时令牌，用户可通过临时令牌访问或使用AWS资源。临时令牌需要制定期限，从15分钟到36小时不等。

角色/临时令牌使用以下场景：

EC2角色——授予对在Amazon EC2实例上运行的应用程序的权限。

跨账户访问——从其他AWS账户向用户授予权限，无论您是否控制这些账户。

联合身份验证——向由受信任的外部系统验证的用户授予权限。



## EC2角色

给应用程序授权是一件很棘手的事。首先，通常在安装时需要根据某种方式来配置应用程序。这回带来安全问题：如何保障配置文件的安全储存以及配置文件不被恶意读取，特别是在安装程序正在运行的时候。

一种替代方法是创建一个IAM角色，并授予对S3存储桶的访问权限。 启动Amazon EC2实例时，将该角色分配给实例。 当在实例上运行的应用程序使用API访问Amazon S3存储桶时，它将以分配的角色获得一个临时令牌，并将其发送给API。 大多数AWS开发工具包都会自动处理获取临时令牌并将其传递给API的过程，从而使应用程序可以进行调用以访问Amazon S3存储桶，而无需担心身份验证。 除了方便开发人员之外，这也消除了将密钥存储在配置文件中的弊端。 



## 跨账户访问

IAM角色另一个常见用法是给其他AWS账户下的IAM用户授予访问资源的权限。这些账户可能是您公司的，也可能是来自外部的代理商，供应商等。 您可把权限集中创建在一个角色里，然后把另一个帐户中的用户放入该角色来访问您的资源。 



## 联合身份验证

许多组织已经在AWS外部建立了身份认证库，它们更倾向于利用原有的认证库，而不是在AWS再重复建设一套。 类似地，基于Web的应用程序可能希望利用基于Web的身份认证，例如Facebook，Google或通过Amazon登录。 IAM身份提供程序提供了将这些与IAM联合并给经过身份验证的外部用户分配特权的功能。

IAM可与两种idP集成。对于基于web的用户，IAM支持通过OpenID Connect集成。对于组织内部用户，如AD或LDAP，IAM支持通过SAML2.0集成。两种集成方式都是通过向角色授予临时令牌来实现身份验证。





# 策略





策略是IAM管理的基础。它是一个JSON格式的字符串，定义了一组访问和操作AWS资源的权限。 策略文档包含一个或多个权限，每个权限定义：

Effect——允许或拒绝；

Service——适用于什么服务？ 大多数AWS Cloud服务都支持通过IAM授予访问权限，包括IAM本身。

Resource——使用的基础设施资源名称（ARN），其基本格式是：

"arn:aws:service:region:account-id:\[resourcetype:\]resource"

Action——操作，允许或拒绝的服务内的操作子集。例如，可以对S3设定读取操作：列举或使用通配符（Read\*）

条件——限制权限允许的操作。例如，将资源的访问权限限制在制定IP地址范围，或者指定时间区间。

下边展示了一个访问策略。 该策略允许用户列出特定存储桶中的对象并检索这些对象，但前提是访问者来自特定的IP地址。

{

"Version": "2012–10–17",

"Statement": \[

{

"Sid": "Stmt1441716043000",

"Effect": "Allow", &lt;- This policy grants access

"Action": \[ &lt;- Allows identities to list

"s3:GetObject", &lt;- and get objects in

"s3:ListBucket" &lt;- the S3 bucket

\],

"Condition": {

"IpAddress": { &lt;- Only from a specific

"aws:SourceIp": "192.168.0.1" &lt;- IP Address

}

},

"Resource": \[

"arn:aws:s3:::my\_public\_bucket/\*" &lt;- Only this bucket

\]

}

\]

}





