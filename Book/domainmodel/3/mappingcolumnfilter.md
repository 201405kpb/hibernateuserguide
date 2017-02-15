#### 2.3.22. @Filter

    <div class="paragraph">

    The `@Filter` annotation is another way to filter out entities or collections using a custom SQL criteria, for both entities and collections.
    Unlike the `@Where` annotation, `@Filter` allows you to parameterize the filter clause at runtime.

    </div>
    <div id="mapping-filter-example" class="exampleblock">
    <div class="title">Example 68. `@Filter` mapping usage</div>
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

        @OneToMany(mappedBy = "client")
        @Filter(name="activeAccount", condition="active = :active")
        private List&lt;Account&gt; accounts = new ArrayList&lt;&gt;( );

        //Getters and setters omitted for brevity

    }

    @Entity(name = "Account")
    @FilterDef(name="activeAccount", parameters=@ParamDef( name="active", type="boolean" ) )
    @Filter(name="activeAccount", condition="active = :active")
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
    <div id="mapping-filter-persistence-example" class="exampleblock">
    <div class="title">Example 69. Persisting an fetching entities with a `@Filter` mapping</div>
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
        client.getAccounts().add( account1 );
        entityManager.persist( account1 );

        Account account2 = new Account( );
        account2.setId( 2L );
        account2.setType( AccountType.DEBIT );
        account2.setAmount( 0d );
        account2.setRate( 1.05 / 100 );
        account2.setActive( false );
        account2.setClient( client );
        client.getAccounts().add( account2 );
        entityManager.persist( account2 );

        Account account3 = new Account( );
        account3.setType( AccountType.DEBIT );
        account3.setId( 3L );
        account3.setAmount( 250d );
        account3.setRate( 1.05 / 100 );
        account3.setActive( true );
        account3.setClient( client );
        client.getAccounts().add( account3 );
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

    By default, without explicitly enabling the filter, Hibernate is going to fetch all `Account` entities.
    If the filter is enabled and the filter parameter value is provided,
    then Hibernate is going to apply the filtering criteria to the associated `Account` entities.

    </div>
    <div id="mapping-filter-entity-query-example" class="exampleblock">
    <div class="title">Example 70. Query entities mapped with `@Filter`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        List&lt;Account&gt; accounts = entityManager.createQuery(
            "select a from Account a", Account.class)
        .getResultList();
        assertEquals( 3, accounts.size());
    } );

    doInJPA( this::entityManagerFactory, entityManager -&gt; {
        log.infof( "Activate filter [%s]", "activeAccount");

        entityManager
            .unwrap( Session.class )
            .enableFilter( "activeAccount" )
            .setParameter( "active", true);

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

    -- Activate filter [activeAccount]

    SELECT
        a.id as id1_0_,
        a.active as active2_0_,
        a.amount as amount3_0_,
        a.client_id as client_i6_0_,
        a.rate as rate4_0_,
        a.account_type as account_5_0_
    FROM
        Account a
    WHERE
        a.active = true`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Jut like with entities, collections can be filtered as well, but only if the filter is explicilty enabled on the currently running Hibernate `Session`.
    This way, when fetching the `accounts` collections, Hibernate is going to apply the `@Filter` clause filtering criteria to the associated collection entries.

    </div>
    <div id="mapping-filter-collection-query-example" class="exampleblock">
    <div class="title">Example 71. Traversing collections mapped with `@Filter`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Client client = entityManager.find( Client.class, 1L );
        assertEquals( 3, client.getAccounts().size() );
    } );

    doInJPA( this::entityManagerFactory, entityManager -&gt; {
        log.infof( "Activate filter [%s]", "activeAccount");

        entityManager
            .unwrap( Session.class )
            .enableFilter( "activeAccount" )
            .setParameter( "active", true);

        Client client = entityManager.find( Client.class, 1L );
        assertEquals( 2, client.getAccounts().size() );
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        c.id as id1_1_0_,
        c.name as name2_1_0_
    FROM
        Client c
    WHERE
        c.id = 1

    SELECT
        a.id as id1_0_,
        a.active as active2_0_,
        a.amount as amount3_0_,
        a.client_id as client_i6_0_,
        a.rate as rate4_0_,
        a.account_type as account_5_0_
    FROM
        Account a
    WHERE
        a.client_id = 1

    -- Activate filter [activeAccount]

    SELECT
        c.id as id1_1_0_,
        c.name as name2_1_0_
    FROM
        Client c
    WHERE
        c.id = 1

    SELECT
        a.id as id1_0_,
        a.active as active2_0_,
        a.amount as amount3_0_,
        a.client_id as client_i6_0_,
        a.rate as rate4_0_,
        a.account_type as account_5_0_
    FROM
        Account a
    WHERE
        accounts0_.active = true
        and a.client_id = 1`</pre>
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

    The main advantage of `@Filter` over the `@Where` clause is that the filtering criteria can be customized at runtime.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock warning">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It&#8217;s not possible to combine the `@Filter` and `@Cache` collection annotations.
    This limitation is due to ensuring consistency and because the filtering information is not stored in the second-level cache.

    </div>
    <div class="paragraph">

    If caching was allowed for a currently filtered collection, then the second-level cache would store only a subset of the whole collection.
    Afterward, every other Session will get the filtered collection from the cache, even if the Session-level filters have not been explicitly activated.

    </div>
    <div class="paragraph">

    For this reason, the second-level collection cache is limited to storing whole collections, and not subsets.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">
