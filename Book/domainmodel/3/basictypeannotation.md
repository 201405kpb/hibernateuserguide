#显示的基础类型

Sometimes you want a particular attribute to be handled differently.Occasionally Hibernate will implicitly pick a `BasicType` that you do not want (and for some reason you do not want to adjust the `BasicTypeRegistry`).
有时你想要一个被不同地处理的特定的属性。 偶尔Hibernate会隐式选择一个你不想要的BasicType（并且由于某种原因你不想调整BasicTypeRegistry）。

In these cases you must explicitly tell Hibernate the `BasicType` to use, via the `org.hibernate.annotations.Type` annotation.
在这些情况下，您必须通过org.hibernate.annotations.Type注释明确告诉Hibernate使用BasicType类型。

_**Example 6. Using `@org.hibernate.annotations.Type`**_
_**例6. 使用`@org.hibernate.annotations.Type`**_
```java
@Entity(name = "Product")
public class Product {

    @Id
    private Integer id;

    private String sku;

    @org.hibernate.annotations.Type( type = "nstring" )
    private String name;

    @org.hibernate.annotations.Type( type = "materialized_nclob" )
    private String description;
}
```
This tells Hibernate to store the Strings as nationalized data.This is just for illustration purposes; for better ways to indicate nationalized character data see [Mapping Nationalized Character Data](/Book/domainmodel/3/basicnationalized.md) section.
这告诉Hibernate将字符串存储为nationalized类型的数据。 这里只是为了说明的目的; 有关nationalized数据的更好方法，请参阅[映射nationalized数据](/Book/domainmodel/3/basicnationalized.md)部分。

Additionally, the description is to be handled as a LOB. Again, for better ways to indicate LOBs see [Mapping LOBs](Book/domainmodel/3/basiclob.md) section.
另外，该描述将作为LOB处理。 再次，为了更好的方式指示LOB，请参阅映射[LOB部分](/Book/domainmodel/3/basiclob.md)。


The `org.hibernate.annotations.Type#type` attribute can name any of the following:
org.hibernate.annotations.Type的type属性可以通过以下任何一个命名：

>* Fully qualified name of any `org.hibernate.type.Type` implementation
>* 任何`org.hibernate.type.Type`实现的完全限定名
>* Any key registered with `BasicTypeRegistry`
>* 使用`BasicTypeRegistry`注册的任何键
>* The name of any known _type definitions_
>* 任何已知类型定义的名称