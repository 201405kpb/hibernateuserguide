### 17.13. Custom SQL for create, update, and delete

    <div class="paragraph">

    Hibernate can use custom SQL for create, update, and delete operations.
    The SQL can be overridden at the statement level or individual column level.
    This section describes statement overrides.
    For columns, see [Column transformers: read and write expressions](#mapping-column-read-and-write).

    </div>
    <div class="paragraph">

    The following example shows how to define custom SQL operations using annotations.
    `@SQLInsert`, `@SQLUpdate` and `@SQLDelete` override the INSERT, UPDATE, DELETE statements of a given entity.
    For the SELECT clause, a `@Loader` must be defined along with a `@NamedNativeQuery` used for loading the underlying table record.

    </div>
    <div class="paragraph">

    For collections, Hibernate allows defining a custom `@SQLDeleteAll` which is used for removing all child records associated with a given parent entity.
    To filter collections, the `@Where` annotation allows customizing the underlying SQL WHERE clause.

    </div>
    <div id="sql-custom-crud-example" class="exampleblock">
    <div class="title">Example 477. Custom CRUD</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    @SQLInsert(
        sql = "INSERT INTO person (name, id, valid) VALUES (?, ?, true) ",
        check = ResultCheckStyle.COUNT
    )
    @SQLUpdate(
        sql = "UPDATE person SET name = ? where id = ? ")
    @SQLDelete(
        sql = "UPDATE person SET valid = false WHERE id = ? ")
    @Loader(namedQuery = "find_valid_person")
    @NamedNativeQueries({
        @NamedNativeQuery(
            name = "find_valid_person",
            query = "SELECT id, name " +
                    "FROM person " +
                    "WHERE id = ? and valid = true",
            resultClass = Person.class
        )
    })
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;

        private String name;

        @ElementCollection
        @SQLInsert(
            sql = "INSERT INTO person_phones (person_id, phones, valid) VALUES (?, ?, true) ")
        @SQLDeleteAll(
            sql = "UPDATE person_phones SET valid = false WHERE person_id = ?")
        @Where( clause = "valid = true" )
        private List&lt;String&gt; phones = new ArrayList&lt;&gt;();

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public List&lt;String&gt; getPhones() {
            return phones;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the example above, the entity is mapped so that entries are soft-deleted (the records are not removed from the database, but instead, a flag marks the row validity).
    The `Person` entity benefits from custom INSERT, UPDATE, and DELETE statements which update the `valid` column accordingly.
    The custom `@Loader` is used to retrieve only `Person` rows that are valid.

    </div>
    <div class="paragraph">

    The same is done for the `phones` collection. The `@SQLDeleteAll` and the `SQLInsert` queries are used whenever the collection is modified.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    You also call a store procedure using the custom CRUD statements; the only requirement is to set the `callable` attribute to `true`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    To check that the execution happens correctly, Hibernate allows you to define one of those three strategies:

    </div>
    <div class="ulist">

*   none: no check is performed; the store procedure is expected to fail upon constraint violations
*   count: use of row-count returned by the `executeUpdate()` method call to check that the update was successful
*   param: like count but using a `CallableStatement` output parameter.
    </div>
    <div class="paragraph">

    To define the result check style, use the `check` parameter.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The parameter order is important and is defined by the order Hibernate handles properties.
    You can see the expected order by enabling debug logging, so Hibernate can print out the static SQL that is used to create, update, delete etc. entities.

    </div>
    <div class="paragraph">

    To see the expected sequence, remember to not include your custom SQL through annotations or mapping files as that will override the Hibernate generated static sql.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Overriding SQL statements for secondary tables is also possible using `@org.hibernate.annotations.Table` and the `sqlInsert`, `sqlUpdate`, `sqlDelete` attributes.

    </div>
    <div id="sql-custom-crud-secondary-table-example" class="exampleblock">
    <div class="title">Example 478. Overriding SQL statements for secondary tables</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    @Table(name = "person")
    @SQLInsert(
        sql = "INSERT INTO person (name, id, valid) VALUES (?, ?, true) "
    )
    @SQLDelete(
        sql = "UPDATE person SET valid = false WHERE id = ? "
    )
    @SecondaryTable(name = "person_details",
        pkJoinColumns = @PrimaryKeyJoinColumn(name = "person_id"))
    @org.hibernate.annotations.Table(
        appliesTo = "person_details",
        sqlInsert = @SQLInsert(
            sql = "INSERT INTO person_details (image, person_id, valid) VALUES (?, ?, true) ",
            check = ResultCheckStyle.COUNT
        ),
        sqlDelete = @SQLDelete(
            sql = "UPDATE person_details SET valid = false WHERE person_id = ? "
        )
    )
    @Loader(namedQuery = "find_valid_person")
    @NamedNativeQueries({
        @NamedNativeQuery(
            name = "find_valid_person",
            query = "select " +
                    "    p.id, " +
                    "    p.name, " +
                    "    pd.image  " +
                    "from person p  " +
                    "left outer join person_details pd on p.id = pd.person_id  " +
                    "where p.id = ? and p.valid = true and pd.valid = true",
            resultClass = Person.class
        )
    })
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;

        private String name;

        @Column(name = "image", table = "person_details")
        private byte[] image;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public byte[] getImage() {
            return image;
        }

        public void setImage(byte[] image) {
            this.image = image;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The SQL is directly executed in your database, so you can use any dialect you like.
    This will, however, reduce the portability of your mapping if you use database specific SQL.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    You can also use stored procedures for customizing the CRUD statements.

    </div>
    <div class="paragraph">

    Assuming the following stored procedure:

    </div>
    <div id="sql-sp-soft-delete-example" class="exampleblock">
    <div class="title">Example 479. Oracle stored procedure to soft-delete a given entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`statement.executeUpdate(
        "CREATE OR REPLACE PROCEDURE sp_delete_person ( " +
        "   personId IN NUMBER ) " +
        "AS  " +
        "BEGIN " +
        "    UPDATE person SET valid = 0 WHERE id = personId; " +
        "END;"
    );}`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The entity can use this stored procedure to soft-delete the entity in question:

    </div>
    <div id="sql-sp-custom-crud-example" class="exampleblock">
    <div class="title">Example 480. Customizing the entity delete statement to use the Oracle stored procedure= instead</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@SQLDelete(
        sql =   "{ call sp_delete_person( ? ) } ",
        callable = true
    )`</pre>
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

    You need to set the `callable` attribute when using a stored procedure instead of an SQL statement.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">