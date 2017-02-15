#@Column注解


JPA defines rules for implicitly determining the name of tables and columns.
For a detailed discussion of implicit naming see [Naming](#naming).


For basic type attributes, the implicit naming rule is that the column name is the same as the attribute name.
If that implicit naming rule does not meet your requirements, you can explicitly tell Hibernate (and other providers) the column name to use.

Example 5. Explicit column naming
```java
@Entity(name = "Product")
public class Product {

    @Id
    private Integer id;

    private String sku;

    private String name;

    @Column( name = "NOTES" )
    private String description;
}
```
    
 Here we use `@Column` to explicitly map the `description` attribute to the `NOTES` column, as opposed to the implicit column name `description`.


The `@Column` annotation defines other mapping information as well. See its Javadocs for details.

  