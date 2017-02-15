# @Base注解

Strictly speaking, a basic type is denoted with with the `javax.persistence.Basic` annotation.
Generally speaking, the `@Basic` annotation can be ignored, as it is assumed by default.
Both of the following examples are ultimately the same.
Example 3. `@Basic` declared explicitly
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
The JPA specification strictly limits the Java types that can be marked as basic to the following listing:


* Java primitive types (`boolean`, `int`, etc)
* wrappers for the primitive types (`java.lang.Boolean`, `java.lang.Integer`, etc)
* `java.lang.String`
* `java.math.BigInteger`
* `java.math.BigDecimal`
* `java.util.Date`
* `java.util.Calendar`
* `java.sql.Date`
* `java.sql.Time`
* `java.sql.Timestamp`
* `byte[]` or `Byte[]`
* `char[]` or `Character[]`
* `enums`
* any other type that implements `Serializable` (JPA&#8217;s "support" for `Serializable` types is to directly serialize their state to the database).


If provider portability is a concern, you should stick to just these basic types.
Note that JPA 2.1 did add the notion of a `javax.persistence.AttributeConverter` to help alleviate some of these concerns; see [JPA 2.1 AttributeConverters](#basic-jpa-convert) for more on this topic.


The `@Basic` annotation defines 2 attributes.


Defines whether this attribute allows nulls.
JPA defines this as "a hint", which essentially means that it effect is specifically required.
As long as the type is not primitive, Hibernate takes this to mean that the underlying column should be `NULLABLE`.

Defines whether this attribute should be fetched eagerly or lazily.
JPA says that EAGER is a requirement to the provider (Hibernate) that the value should be fetched when the owner is fetched, while LAZY is merely a hint that the value be fetched when the attribute is accessed.
Hibernate ignores this setting for basic types unless you are using bytecode enhancement.
See the [BytecodeEnhancement](#BytecodeEnhancement) for additional information on fetching and on bytecode enhancement.

