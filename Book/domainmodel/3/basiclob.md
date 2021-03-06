 #### 2.3.8. LOBs 类型映射

Mapping LOBs (database Large Objects) come in 2 forms, those using the JDBC locator types and those materializing the LOB data.

映射LOB（数据库大对象）有2种形式，使用JDBC定位器类型和实现LOB数据的那些类型。

JDBC LOB locators exist to allow efficient access to the LOB data.They allow the JDBC driver to stream parts of the LOB data as needed, potentially freeing up memory space.However they can be unnatural to deal with and have certain limitations.For example, a LOB locator is only portably valid during the duration of the transaction in which it was obtained.

JDBC LOB定位器可以有效地有效访问LOB数据。 它们允许JDBC驱动程序根据需要对LOB数据的一部分进行流式传输，从而可能释放内存空间。 然而，他们可能是不自然的处理和有一定的限制。 例如，LOB定位器仅在其被获得的事务的持续时间期间内可移植是有效的。

The idea of materialized LOBs is to trade-off the potential efficiency (not all drivers handle LOB data efficiently) for a more natural programming paradigm using familiar Java types such as String or byte[], etc for these LOBs.

物化LOB的想法是利用熟悉的Java类型（如String或byte []等）为这些LOB平衡潜在的效率（并非所有驱动程序都能有效地处理LOB数据）。

Materialized deals with the entire LOB contents in memory, whereas LOB locators (in theory) allow streaming parts of the LOB contents into memory as needed.

理论上，LOB定位器允许根据需要将LOB内容的流部分分配内存到内存中，实现处理内存中的整个LOB内容。

The JDBC LOB locator types include:
JBDBC定位器类型包括：

*   `java.sql.Blob`
*   `java.sql.Clob`
*   `java.sql.NClob`
   
Mapping materialized forms of these LOB values would use more familiar Java types such as `String`, `char[]`, `byte[]`, etc. The trade off for _more familiar_ is usually performance.

映射这些LOB值的物化形式将使用更熟悉的Java类型，例如`String`，`char []`，`byte []`等。 更加熟悉的权衡标准通常是性能。


For a first look, let&#8217;s assume we have a `CLOB` column that we would like to map (`NCLOB` character `LOB` data will be covered in [Mapping Nationalized Character Data](/Book/domainmodel/3/basicnationalized.md) section).
首先，让我们假设我们有一个想要映射的“CLOB”列，（`NCLOB`字符`LOB`数据将在[Mapping Nationalized Character Data]（/Book/domainmodel/3/basicnationalized.md）部分进行详细介绍）。


    </div>
    <div id="basic-clob-sql-example" class="exampleblock">
    <div class="title">Example 25. CLOB - SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Product (
      id INTEGER NOT NULL
      image clob
      name VARCHAR(255)
      PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Let&#8217;s first map this using the `@Lob` JPA annotation and the `java.sql.Clob` type:

    </div>
    <div id="basic-clob-example" class="exampleblock">
    <div class="title">Example 26. `CLOB` mapped to `java.sql.Clob`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        private Clob warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To persist such an entity, you have to create a `Clob` using plain JDBC:

    </div>
    <div id="basic-clob-persist-example" class="exampleblock">
    <div class="title">Example 27. Persisting a `java.sql.Clob` entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`String warranty = "My product warranty";

    final Product product = new Product();
    product.setId( 1 );
    product.setName( "Mobile phone" );

    session.doWork( connection -&gt; {
        product.setWarranty( ClobProxy.generateProxy( warranty ) );
    } );

    entityManager.persist( product );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To retrieve the `Clob` content, you need to transform the underlying `java.io.Reader`:

    </div>
    <div id="basic-clob-find-example" class="exampleblock">
    <div class="title">Example 28. Returning a `java.sql.Clob` entity</div>
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

    We could also map the CLOB in a materialized form. This way, we can either use a `String` or a `char[]`.

    </div>
    <div id="basic-clob-string-example" class="exampleblock">
    <div class="title">Example 29. `CLOB` mapped to `String`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        private String warranty;

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

    How JDBC deals with `LOB` data varies from driver to driver, and Hibernate tries to handle all these variances on your behalf.

    </div>
    <div class="paragraph">

    However, some drivers are trickier (e.g. PostgreSQL JDBC drivers), and, in such cases, you may have to do some extra to get LOBs working.
    Such discussions are beyond the scope of this guide.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    We might even want the materialized data as a char array (for some crazy reason).

    </div>
    <div id="basic-clob-char-array-example" class="exampleblock">
    <div class="title">Example 30. CLOB - materialized `char[]` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        private char[] warranty;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    `BLOB` data is mapped in a similar fashion.

    </div>
    <div id="basic-blob-sql-example" class="exampleblock">
    <div class="title">Example 31. BLOB - SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Product (
        id INTEGER NOT NULL ,
        image blob ,
        name VARCHAR(255) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Let&#8217;s first map this using the JDBC `java.sql.Blob` type.

    </div>
    <div id="basic-blob-example" class="exampleblock">
    <div class="title">Example 32. `BLOB` mapped to `java.sql.Blob`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        private Blob image;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To persist such an entity, you have to create a `Blob` using plain JDBC:

    </div>
    <div id="basic-blob-persist-example" class="exampleblock">
    <div class="title">Example 33. Persisting a `java.sql.Blob` entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`byte[] image = new byte[] {1, 2, 3};

    final Product product = new Product();
    product.setId( 1 );
    product.setName( "Mobile phone" );

    session.doWork( connection -&gt; {
        product.setImage( BlobProxy.generateProxy( image ) );
    } );

    entityManager.persist( product );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To retrieve the `Blob` content, you need to transform the underlying `java.io.Reader`:

    </div>
    <div id="basic-blob-find-example" class="exampleblock">
    <div class="title">Example 34. Returning a `java.sql.Blob` entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Product product = entityManager.find( Product.class, productId );

    try (InputStream inputStream = product.getImage().getBinaryStream()) {
        assertArrayEquals(new byte[] {1, 2, 3}, toBytes( inputStream ) );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We could also map the BLOB in a materialized form (e.g. `byte[]`).

    </div>
    <div id="basic-blob-byte-array-example" class="exampleblock">
    <div class="title">Example 35. `BLOB` mapped to `byte[]`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Integer id;

        private String name;

        @Lob
        private byte[] image;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">