#### 2.3.17. SQL quoted identifiers

    <div class="paragraph">

    You can force Hibernate to quote an identifier in the generated SQL by enclosing the table or column name in backticks in the mapping document.
    While traditionally, Hibernate used backticks for escaping SQL reserved keywords, JPA uses double quotes instead.

    </div>
    <div class="paragraph">

    Once the reserved keywords are escaped, Hibernate will use the correct quotation style for the SQL `Dialect`.
    This is usually double quotes, but SQL Server uses brackets and MySQL uses backticks.

    </div>
    <div id="basic-quoting-example" class="exampleblock">
    <div class="title">Example 51. Hibernate legacy quoting</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Long id;

        @Column(name = "`name`")
        private String name;

        @Column(name = "`number`")
        private String number;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="basic-jpa-quoting-example" class="exampleblock">
    <div class="title">Example 52. JPA quoting</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Long id;

        @Column(name = "\"name\"")
        private String name;

        @Column(name = "\"number\"")
        private String number;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Because `name` and `number` are reserved words, the `Product` entity mapping uses backtricks to quote these column names.

    </div>
    <div class="paragraph">

    When saving the following `Product entity`, Hibernate generates the following SQL insert statement:

    </div>
    <div id="basic-quoting-persistence-example" class="exampleblock">
    <div class="title">Example 53. Persisting a quoted column name</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Product product = new Product();
    product.setId( 1L );
    product.setName( "Mobile phone" );
    product.setNumber( "123-456-7890" );
    entityManager.persist( product );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Product ("name", "number", id)
    VALUES ('Mobile phone', '123-456-7890', 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Global quoting

    <div class="paragraph">

    Hibernate can also quote all identifiers (e.g. table, columns) using the following configuration property:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.globally_quoted_identifiers"
        value="true"
    /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This way, we don&#8217;t need to manually quote any identifier:

    </div>
    <div id="basic-auto-quoting-example" class="exampleblock">
    <div class="title">Example 54. JPA quoting</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Product")
    public static class Product {

        @Id
        private Long id;

        private String name;

        private String number;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When persisting a `Product` entity, Hibernate is going to quote all identifiers as in the following example:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO "Product" ("name", "number", "id")
    VALUES ('Mobile phone', '123-456-7890', 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see, both the table name and all the column have been quoted.

    </div>
    <div class="paragraph">

    For more about quoting-related configuration properties, check out the [Mapping configurations](#configurations-mapping) section as well.

    </div>
    </div>
    </div>
    <div class="sect3">