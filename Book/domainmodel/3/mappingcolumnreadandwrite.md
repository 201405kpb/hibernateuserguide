#### 2.3.19. Column transformers: read and write expressions

    <div class="paragraph">

    Hibernate allows you to customize the SQL it uses to read and write the values of columns mapped to `@Basic` types.
    For example, if your database provides a set of data encryption functions, you can invoke them for individual columns like in the following example.

    </div>
    <div id="mapping-column-read-and-write-example" class="exampleblock">
    <div class="title">Example 59. `@ColumnTransformer` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Employee")
    public static class Employee {

        @Id
        private Long id;

        @NaturalId
        private String username;

        @Column(name = "pswd")
        @ColumnTransformer(
            read = "decrypt( 'AES', '00', pswd  )",
            write = "encrypt('AES', '00', ?)"
        )
        private String password;

        private int accessLevel;

        @ManyToOne(fetch = FetchType.LAZY)
        private Department department;

        @ManyToMany(mappedBy = "employees")
        private List&lt;Project&gt; projects = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity
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

    You can use the plural form `@ColumnTransformers` if more than one columns need to define either of these rules.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    If a property uses more than one column, you must use the `forColumn` attribute to specify which column, the expressions are targeting.

    </div>
    <div id="mapping-column-read-and-write-composite-type-example" class="exampleblock">
    <div class="title">Example 60. `@ColumnTransformer` `forColumn` attribute usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Savings")
    public static class Savings {

        @Id
        private Long id;

        @Type(type = "org.hibernate.userguide.mapping.basic.MonetaryAmountUserType")
        @Columns(columns = {
            @Column(name = "money"),
            @Column(name = "currency")
        })
        @ColumnTransformer(
            forColumn = "money",
            read = "money / 100",
            write = "? * 100"
        )
        private MonetaryAmount wallet;

        //Getters and setters omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate applies the custom expressions automatically whenever the property is referenced in a query.
    This functionality is similar to a derived-property [@Formula](#mapping-column-formula) with two differences:

    </div>
    <div class="ulist">

*   The property is backed by one or more columns that are exported as part of automatic schema generation.
*   The property is read-write, not read-only.
    </div>
    <div class="paragraph">

    The `write` expression, if specified, must contain exactly one '?' placeholder for the value.

    </div>
    <div id="mapping-column-read-and-write-composite-type-persistence-example" class="exampleblock">
    <div class="title">Example 61. Persisting an entity with a `@ColumnTransformer` and a composite type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Savings savings = new Savings( );
        savings.setId( 1L );
        savings.setWallet( new MonetaryAmount( BigDecimal.TEN, Currency.getInstance( Locale.US ) ) );
        entityManager.persist( savings );
    } );

    doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Savings savings = entityManager.find( Savings.class, 1L );
        assertEquals( 10, savings.getWallet().getAmount().intValue());
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Savings (money, currency, id)
    VALUES (10 * 100, 'USD', 1)

    SELECT
        s.id as id1_0_0_,
        s.money / 100 as money2_0_0_,
        s.currency as currency3_0_0_
    FROM
        Savings s
    WHERE
        s.id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">