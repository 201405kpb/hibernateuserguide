# @Base注解

Strictly speaking, a basic type is denoted with with the `javax.persistence.Basic` annotation.
Generally speaking, the `@Basic` annotation can be ignored, as it is assumed by default.
Both of the following examples are ultimately the same.
严格地说，一个基本类型用`javax.persistence.Basic`注释表示。一般来说，`@ Basic`注释可以忽略，因为它是默认的。以下两个实施例都是相同的。

_**Example 3. `@Basic` declared explicitly**_
_**例3 明确声明 `@Basic`注解**_
```java
@Entity(name = "Product")
public class Product {

    @Id
    @Basic
    private Integer id;

    @Basic
    private String sku;

    @Basic
    private String name;

    @Basic
    private String description;
}
```

Example 4. `@Basic` being implicitly implied
```java
@Entity(name = "Product")
public class Product {

    @Id
    private Integer id;

    private String sku;

    private String name;

    private String description;
}
```
>![Basic Note](/Book/images/org/hibernate/docbook/note.png)
>The JPA specification strictly limits the Java types that can be marked as basic to the following listing:
>JPA规范严格限制可以标记为基本的Java类型到以下列表：

>* Java primitive types (`boolean`, `int`, etc)
* Java 原始类型（例如 `boolean`, `int`等）
* wrappers for the primitive types (`java.lang.Boolean`, `java.lang.Integer`, etc)
* 原始类型的封装器（例如 `java.lang.Boolean`, `java.lang.Integer`等 ）
* `java.lang.String`
* `java.lang.String`类型
* `java.math.BigInteger`
* `java.math.BigInteger`类型
* `java.math.BigDecimal`
* `java.math.BigDecimal`类型
* `java.util.Date`
* `java.util.Date` 类型
* `java.util.Calendar`
* `java.util.Calendar`类型
* `java.sql.Date`
* `java.sql.Date`类型
* `java.sql.Time`
* `java.sql.Time` 类型
* `java.sql.Timestamp`
* `java.sql.Timestamp`类型
* `byte[]` or `Byte[]`
* `byte[]`类型或 `Byte[]`类型
* `char[]` or `Character[]`
* `char[]`类型或 `Character[]`类型
* `enums`
* `enums` 类型
* any other type that implements `Serializable` (JPA&#8217;s "support" for `Serializable` types is to directly serialize their state to the database).
* 任何其他实现“Serializable”的类型（JPA对“Serializable”类型的“支持”是直接将其状态序列化到数据库）。

>If provider portability is a concern, you should stick to just these basic types.
Note that JPA 2.1 did add the notion of a `javax.persistence.AttributeConverter` to help alleviate some of these concerns; see [JPA 2.1 AttributeConverters](#basic-jpa-convert) for more on this topic.
如果提供者的可移植性是一个问题，你应该坚持只是这些基本类型。注意JPA 2.1添加了一个`javax.persistence.AttributeConverter`的概念来帮助缓解这些问题; 有关此主题的更多信息，请参阅[JPA 2.1 AttributeConverters](＃basic-jpa-convert)。


The `@Basic` annotation defines 2 attributes.


Defines whether this attribute allows nulls.
JPA defines this as "a hint", which essentially means that it effect is specifically required.
As long as the type is not primitive, Hibernate takes this to mean that the underlying column should be `NULLABLE`.

Defines whether this attribute should be fetched eagerly or lazily.
JPA says that EAGER is a requirement to the provider (Hibernate) that the value should be fetched when the owner is fetched, while LAZY is merely a hint that the value be fetched when the attribute is accessed.
Hibernate ignores this setting for basic types unless you are using bytecode enhancement.
See the [BytecodeEnhancement](#BytecodeEnhancement) for additional information on fetching and on bytecode enhancement.

