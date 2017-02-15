 #### 2.3.21. @Where

    <div class="paragraph">

    Sometimes, you want to filter out entities or collections using a custom SQL criteria.
    This can be achieved using the `@Where` annotation, which can be applied to entities and collections.

    </div>
    <div id="mapping-where-example" class="exampleblock">
    <div class="title">Example 64. `@Where` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public enum AccountType {
        DEBIT,
        CREDIT
    }

    @Entity(name = "Client")
    public static class Client {

        @Id
        private Long id;

        private String name;

        @Where( clause = "account_type = 'DEBIT'")
        @OneToMany(mappedBy = "client")
        private List&lt;Account&gt; debitAccounts = new ArrayList&lt;&gt;( );

        @Where( clause = "account_type = 'CREDIT'")
        @OneToMany(mappedBy = "client")
        private List&lt;Account&gt; creditAccounts = new ArrayList&lt;&gt;( );

        //Getters and setters omitted for brevity

    }

    @Entity(name = "Account")
    @Where( clause = "active = true" )
    public static class Account {

        @Id
        private Long id;

        @ManyToOne
        private Client client;

        @Column(name = "account_type")
        @Enumerated(EnumType.STRING)
        private AccountType type;

        private Double amount;

        private Double rate;

        private boolean active;

        //Getters and setters omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If the database contains the following entities:

    </div>
    <div id="mapping-where-persistence-example" class="exampleblock">
    <div class="title">Example 65. Persisting an fetching entities with a `@Where` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {

        Client client = new Client();
        client.setId( 1L );
        client.setName( "John Doe" );
        entityManager.persist( client );

        Account account1 = new Account( );
        account1.setId( 1L );
        account1.setType( AccountType.CREDIT );
        account1.setAmount( 5000d );
        account1.setRate( 1.25 / 100 );
        account1.setActive( true );
        account1.setClient( client );
        client.getCreditAccounts().add( account1 );
        entityManager.persist( account1 );

        Account account2 = new Account( );
        account2.setId( 2L );
        account2.setType( AccountType.DEBIT );
        account2.setAmount( 0d );
        account2.setRate( 1.05 / 100 );
        account2.setActive( false );
        account2.setClient( client );
        client.getDebitAccounts().add( account2 );
        entityManager.persist( account2 );

        Account account3 = new Account( );
        account3.setType( AccountType.DEBIT );
        account3.setId( 3L );
        account3.setAmount( 250d );
        account3.setRate( 1.05 / 100 );
        account3.setActive( true );
        account3.setClient( client );
        client.getDebitAccounts().add( account3 );
        entityManager.persist( account3 );
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Client (name, id)
    VALUES ('John Doe', 1)

    INSERT INTO Account (active, amount, client_id, rate, account_type, id)
    VALUES (true, 5000.0, 1, 0.0125, 'CREDIT', 1)

    INSERT INTO Account (active, amount, client_id, rate, account_type, id)
    VALUES (false, 0.0, 1, 0.0105, 'DEBIT', 2)

    INSERT INTO Account (active, amount, client_id, rate, account_type, id)
    VALUES (true, 250.0, 1, 0.0105, 'DEBIT', 3)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When executing an `Account` entity query, Hibernate is going to filter out all records that are not active.

    </div>
    <div id="mapping-where-entity-query-example" class="exampleblock">
    <div class="title">Example 66. Query entities mapped with `@Where`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        List&lt;Account&gt; accounts = entityManager.createQuery(
            "select a from Account a", Account.class)
        .getResultList();
        assertEquals( 2, accounts.size());
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        a.id as id1_0_,
        a.active as active2_0_,
        a.amount as amount3_0_,
        a.client_id as client_i6_0_,
        a.rate as rate4_0_,
        a.account_type as account_5_0_
    FROM
        Account a
    WHERE ( a.active = true )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When fetching the `debitAccounts` or the `creditAccounts` collections, Hibernate is going to apply the `@Where` clause filtering criteria to the associated child entities.

    </div>
    <div id="mapping-where-collection-query-example" class="exampleblock">
    <div class="title">Example 67. Traversing collections mapped with `@Where`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Client client = entityManager.find( Client.class, 1L );
        assertEquals( 1, client.getCreditAccounts().size() );
        assertEquals( 1, client.getDebitAccounts().size() );
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        c.client_id as client_i6_0_0_,
        c.id as id1_0_0_,
        c.id as id1_0_1_,
        c.active as active2_0_1_,
        c.amount as amount3_0_1_,
        c.client_id as client_i6_0_1_,
        c.rate as rate4_0_1_,
        c.account_type as account_5_0_1_
    FROM
        Account c
    WHERE ( c.active = true and c.account_type = 'CREDIT' ) AND c.client_id = 1

    SELECT
        d.client_id as client_i6_0_0_,
        d.id as id1_0_0_,
        d.id as id1_0_1_,
        d.active as active2_0_1_,
        d.amount as amount3_0_1_,
        d.client_id as client_i6_0_1_,
        d.rate as rate4_0_1_,
        d.account_type as account_5_0_1_
    FROM
        Account d
    WHERE ( d.active = true and d.account_type = 'DEBIT' ) AND d.client_id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">