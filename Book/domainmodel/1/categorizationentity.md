# 实体类型

Entities, by nature of their unique identifier, exist independently of other objects whereas values do not.Entities are domain model classes which correlate to rows in a database table, using a unique identifier.Because of the requirement for a unique identifier, entities exist independently and define their own lifecycle.
实体类型，由于其唯一标识符的性质，是独立于其他对象存在的，而值类型却不是。实体是使用唯一标识符与数据库表中的行相关联的领域模型类。由于对唯一标识符的要求，实体独立存在并且定义它们自己的生命周期。

The `Contact` class itself would be an example of an entity.
`Contact`类本身就是一个实体的例子。

Mapping entities is discussed in detail in [Entity](/Book/domainmodel/5/entity.md).
实体映射将在[这里](/Book/domainmodel/5/entity.md)进行详细讨论。
 