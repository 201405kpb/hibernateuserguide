#@Column注解


JPA defines rules for implicitly determining the name of tables and columns.For a detailed discussion of implicit naming see [Naming](/Book/domainmodel/2/naming.md).
JPA定义用于隐式确定表和列的名称的规则。 有关隐式命名的详细讨论，请参阅[命名](/Book/domainmodel/2/naming.md)。

For basic type attributes, the implicit naming rule is that the column name is the same as the attribute name.If that implicit naming rule does not meet your requirements, you can explicitly tell Hibernate (and other providers) the column name to use.
对于基本类型属性，隐式命名规则是列名称与属性名称相同。如果隐式命名规则不符合您的要求，您可以明确告诉Hibernate（和其他提供程序）要使用的列名称。

_**Example 5. Explicit column naming**_
_**例5. 显式列命名**_
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
这里我们使用`@ Column`来将`description`属性显式映射到`NOTES`列，而不是隐式列名`description`列。

The `@Column` annotation defines other mapping information as well. See its Javadocs for details.
`@ Column`注解也定义了其他映射信息。 有关详细信息，请参阅其Javadoc。

  