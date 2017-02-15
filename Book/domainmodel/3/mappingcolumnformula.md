#### 2.3.20. @Formula

    <div class="paragraph">

    Sometimes, you want the Database to do some computation for you rather than in the JVM, you might also create some kind of virtual column.
    You can use a SQL fragment (aka formula) instead of mapping a property into a column. This kind of property is read only (its value is calculated by your formula fragment)

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    You should be aware that the `@Formula` annotation takes a native SQL clause which can affect database portability.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="mapping-column-formula-example" class="exampleblock">
    <div class="title">Example 62. `@Formula` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    public static class Account {

        @Id
        private Long id;

        private Double credit;

        private Double rate;

        @Formula(value = "credit * rate")
        private Double interest;

        //Getters and setters omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When loading the `Account` entity, Hibernate is going to calculate the `interest` property using the configured `@Formula`:

    </div>
    <div id="mapping-column-formula-persistence-example" class="exampleblock">
    <div class="title">Example 63. Persisting an entity with a `@Formula` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Account account = new Account( );
        account.setId( 1L );
        account.setCredit( 5000d );
        account.setRate( 1.25 / 100 );
        entityManager.persist( account );
    } );

    doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Account account = entityManager.find( Account.class, 1L );
        assertEquals( Double.valueOf( 62.5d ), account.getInterest());
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Account (credit, rate, id)
    VALUES (5000.0, 0.0125, 1)

    SELECT
        a.id as id1_0_0_,
        a.credit as credit2_0_0_,
        a.rate as rate3_0_0_,
        a.credit * a.rate as formula0_0_
    FROM
        Account a
    WHERE
        a.id = 1`</pre>
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

    The SQL fragment can be as complex as you want and even include subselects.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">