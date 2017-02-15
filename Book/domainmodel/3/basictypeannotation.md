#Explicit BasicTypes

Sometimes you want a particular attribute to be handled differently.
Occasionally Hibernate will implicitly pick a `BasicType` that you do not want (and for some reason you do not want to adjust the `BasicTypeRegistry`).



In these cases you must explicitly tell Hibernate the `BasicType` to use, via the `org.hibernate.annotations.Type` annotation.
Example 6. Using `@org.hibernate.annotations.Type`
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

This tells Hibernate to store the Strings as nationalized data.
This is just for illustration purposes; for better ways to indicate nationalized character data see [Mapping Nationalized Character Data](#basic-nationalized) section.

Additionally, the description is to be handled as a LOB. Again, for better ways to indicate LOBs see [Mapping LOBs](#basic-lob) section.


The `org.hibernate.annotations.Type#type` attribute can name any of the following:

* Fully qualified name of any `org.hibernate.type.Type` implementation
* Any key registered with `BasicTypeRegistry`
* The name of any known _type definitions_