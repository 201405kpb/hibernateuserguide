#域模型

The term [domain model](https://en.wikipedia.org/wiki/Domain_model) comes from the realm of data modeling. 
It is the model that ultimately describes the [problem domain](https://en.wikipedia.org/wiki/Problem_domain) you are working in.Sometimes you will also hear the term persistent classes.
[领域模型](https://en.wikipedia.org/wiki/Domain_model)是一个术语，来自数据建模领域。
它最终描述你正在工作的[问题域](https://en.wikipedia.org/wiki/Problem_domain)的模型。
有时也会称领域模型为持久化类。

Ultimately the application domain model is the central character in an ORM.
They make up the classes you wish to map. Hibernate works best if these classes follow the Plain Old Java Object (POJO) / JavaBean programming model.
However, none of these rules are hard requirements.
Indeed, Hibernate assumes very little about the nature of your persistent objects. You can express a domain model in other ways (using trees of `java.util.Map` instances, for example).

最终，应用程序域模型是ORM中的中心特性。他们组成你想要映射的类。如果这些类遵循普通Java对象（PO​​JO）/ JavaBean编程模型，Hibernate的工作效果最好。然而，这些规则都不是硬性要求。事实上，Hibernate对于持久化对象的本质并不太了解。你可以用其他方式表达域模型（例如使用`java.util.Map`实例的树）。

Historically applications using Hibernate would have used its proprietary XML mapping file format for this purpose.With the coming of JPA, most of this information is now defined in a way that is portable across ORM/JPA providers using annotations (and/or standardized XML format).
This chapter will focus on JPA mapping where possible.For Hibernate mapping features not supported by JPA we will prefer Hibernate extension annotations.

历史上，使用Hibernate的应用程序将使用其专有的XML映射文件格式来实现此目的。随着JPA的到来，这些信息中的大多数现在被定义为使用注释（和/或标准化XML格式）在ORM / JPA提供者之间可移植的方式。
本章将尽可能关注JPA映射。对于JPA不支持的Hibernate映射功能，我们更喜欢Hibernate扩展注释。