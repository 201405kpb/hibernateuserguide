#### 2.3.9. Mapping Nationalized Character Data

    <div class="paragraph">

    JDBC 4 added the ability to explicitly handle nationalized character data.
    To this end it added specific nationalized character data types.

    </div>
    <div class="ulist">

*   `NCHAR`
*   `NVARCHAR`
*   `LONGNVARCHAR`
*   `NCLOB`
    </div>
    <div id="basic-nationalized-sql-example" class="exampleblock">
    <div class="title">Example 36. `NVARCHAR` - SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Product (
        id INTEGER NOT NULL ,
        name VARCHAR(255) ,
        warranty NVARCHAR(255) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To map a specific attribute to a nationalized variant data type, Hibernate defines the `@Nationalized` annotation.

    </div>
    <div id="basic-nationalized-example" class="exampleblock">
    <div class="title">Example 37. `NVARCHAR` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Nationalized
        private String warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Just like with `CLOB`, Hibernate can also deal with `NCLOB` SQL data types:

    </div>
    <div id="basic-nclob-sql-example" class="exampleblock">
    <div class="title">Example 38. `NCLOB` - SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Product (
        id INTEGER NOT NULL ,
        name VARCHAR(255) ,
        warranty nclob ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate can map the `NCLOB` to a `java.sql.NClob`

    </div>
    <div id="basic-nclob-example" class="exampleblock">
    <div class="title">Example 39. `NCLOB` mapped to `java.sql.NClob`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        @Nationalized
        // Clob also works, because NClob extends Clob.
        // The database type is still NCLOB either way and handled as such.
        private NClob warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To persist such an entity, you have to create a `NClob` using plain JDBC:

    </div>
    <div id="basic-nclob-persist-example" class="exampleblock">
    <div class="title">Example 40. Persisting a `java.sql.NClob` entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`String warranty = "My product warranty";

    final Product product = new Product();
    product.setId( 1 );
    product.setName( "Mobile phone" );

    session.doWork( connection -&gt; {
        product.setWarranty( connection.createNClob() );
        product.getWarranty().setString( 1, warranty );
    } );

    entityManager.persist( product );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To retrieve the `NClob` content, you need to transform the underlying `java.io.Reader`:

    </div>
    <div id="basic-nclob-find-example" class="exampleblock">
    <div class="title">Example 41. Returning a `java.sql.NClob` entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Product product = entityManager.find( Product.class, productId );
    try (Reader reader = product.getWarranty().getCharacterStream()) {
        assertEquals( "My product warranty", toString( reader ) );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We could also map the `NCLOB` in a materialized form. This way, we can either use a `String` or a `char[]`.

    </div>
    <div id="basic-nclob-string-example" class="exampleblock">
    <div class="title">Example 42. `NCLOB` mapped to `String`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        @Nationalized
        private String warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We might even want the materialized data as a char array.

    </div>
    <div id="basic-nclob-char-array-example" class="exampleblock">
    <div class="title">Example 43. NCLOB - materialized `char[]` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        @Nationalized
        private char[] warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you application and database are entirely nationalized you may instead want to enable nationalized character data as the default.
    You can do this via the `hibernate.use_nationalized_character_data` setting or by calling `MetadataBuilder#enableGlobalNationalizedCharacterDataSupport` during bootstrap.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">