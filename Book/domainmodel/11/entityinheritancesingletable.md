#### 2.11.2. Single table

    <div class="paragraph">

    The single table inheritance strategy maps all subclasses to only one database table.
    Each subclass declares its own persistent properties.
    Version and id properties are assumed to be inherited from the root class.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    When omitting an explicit inheritance strategy (e.g. `@Inheritance`), JPA will choose the `SINGLE_TABLE` strategy by default.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="entity-inheritance-single-table-example" class="exampleblock">
    <div class="title">Example 182. Single Table inheritance</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    public static class Account {

        @Id
        private Long id;

        private String owner;

        private BigDecimal balance;

        private BigDecimal interestRate;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getOwner() {
            return owner;
        }

        public void setOwner(String owner) {
            this.owner = owner;
        }

        public BigDecimal getBalance() {
            return balance;
        }

        public void setBalance(BigDecimal balance) {
            this.balance = balance;
        }

        public BigDecimal getInterestRate() {
            return interestRate;
        }

        public void setInterestRate(BigDecimal interestRate) {
            this.interestRate = interestRate;
        }
    }

    @Entity(name = "DebitAccount")
    public static class DebitAccount extends Account {

        private BigDecimal overdraftFee;

        public BigDecimal getOverdraftFee() {
            return overdraftFee;
        }

        public void setOverdraftFee(BigDecimal overdraftFee) {
            this.overdraftFee = overdraftFee;
        }
    }

    @Entity(name = "CreditAccount")
    public static class CreditAccount extends Account {

        private BigDecimal creditLimit;

        public BigDecimal getCreditLimit() {
            return creditLimit;
        }

        public void setCreditLimit(BigDecimal creditLimit) {
            this.creditLimit = creditLimit;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Account (
        DTYPE VARCHAR(31) NOT NULL ,
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        overdraftFee NUMERIC(19, 2) ,
        creditLimit NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Each subclass in a hierarchy must define a unique discriminator value, which is used to differentiate between rows belonging to separate subclass types.
    If this is not specified, the `DTYPE` column is used as a discriminator, storing the associated subclass name.

    </div>
    <div id="entity-inheritance-single-table-persist-example" class="exampleblock">
    <div class="title">Example 183. Single Table inheritance discriminator column</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DebitAccount debitAccount = new DebitAccount();
    debitAccount.setId( 1L );
    debitAccount.setOwner( "John Doe" );
    debitAccount.setBalance( BigDecimal.valueOf( 100 ) );
    debitAccount.setInterestRate( BigDecimal.valueOf( 1.5d ) );
    debitAccount.setOverdraftFee( BigDecimal.valueOf( 25 ) );

    CreditAccount creditAccount = new CreditAccount();
    creditAccount.setId( 2L );
    creditAccount.setOwner( "John Doe" );
    creditAccount.setBalance( BigDecimal.valueOf( 1000 ) );
    creditAccount.setInterestRate( BigDecimal.valueOf( 1.9d ) );
    creditAccount.setCreditLimit( BigDecimal.valueOf( 5000 ) );

    entityManager.persist( debitAccount );
    entityManager.persist( creditAccount );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Account (balance, interestRate, owner, overdraftFee, DTYPE, id)
    VALUES (100, 1.5, 'John Doe', 25, 'DebitAccount', 1)

    INSERT INTO Account (balance, interestRate, owner, creditLimit, DTYPE, id)
    VALUES (1000, 1.9, 'John Doe', 5000, 'CreditAccount', 2)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using polymorphic queries, only a single table is required to be scanned to fetch all associated subclass instances.

    </div>
    <div id="entity-inheritance-single-table-query-example" class="exampleblock">
    <div class="title">Example 184. Single Table polymorphic query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Account&gt; accounts = entityManager
        .createQuery( "select a from Account a" )
        .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT  singletabl0_.id AS id2_0_ ,
            singletabl0_.balance AS balance3_0_ ,
            singletabl0_.interestRate AS interest4_0_ ,
            singletabl0_.owner AS owner5_0_ ,
            singletabl0_.overdraftFee AS overdraf6_0_ ,
            singletabl0_.creditLimit AS creditLi7_0_ ,
            singletabl0_.DTYPE AS DTYPE1_0_
    FROM    Account singletabl0_`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Among all other inheritance alternatives, the single table strategy performs the best since it requires access to one table only.
    Because all subclass columns are stored in a single table, it&#8217;s not possible to use NOT NULL constraints anymore, so integrity checks must be moved either into the data access layer or enforced through `CHECK` or `TRIGGER` constraints.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect4">

    ##### Discriminator

    <div class="paragraph">

    The discriminator column contains marker values that tell the persistence layer what subclass to instantiate for a particular row.
    Hibernate Core supports the following restricted set of types as discriminator column: `String`, `char`, `int`, `byte`, `short`, `boolean`(including `yes_no`, `true_false`).

    </div>
    <div class="paragraph">

    Use the `@DiscriminatorColumn` to define the discriminator column as well as the discriminator type.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The enum `DiscriminatorType` used in `javax.persistence.DiscriminatorColumn` only contains the values `STRING`, `CHAR` and `INTEGER` which means that not all Hibernate supported types are available via the `@DiscriminatorColumn` annotation.
    You can also use `@DiscriminatorFormula` to express in SQL a virtual discriminator column.
    This is particularly useful when the discriminator value can be extracted from one or more columns of the table.
    Both `@DiscriminatorColumn` and `@DiscriminatorFormula` are to be set on the root entity (once per persisted hierarchy).

    </div>
    <div class="paragraph">

    `@org.hibernate.annotations.DiscriminatorOptions` allows to optionally specify Hibernate specific discriminator options which are not standardized in JPA.
    The available options are `force` and `insert`.

    </div>
    <div class="paragraph">

    The `force` attribute is useful if the table contains rows with _extra_ discriminator values that are not mapped to a persistent class.
    This could for example occur when working with a legacy database.
    If `force` is set to true Hibernate will specify the allowed discriminator values in the SELECT query, even when retrieving all instances of the root class.

    </div>
    <div class="paragraph">

    The second option, `insert`, tells Hibernate whether or not to include the discriminator column in SQL INSERTs.
    Usually, the column should be part of the INSERT statement, but if your discriminator column is also part of a mapped composite identifier you have to set this option to false.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    There used to be `@org.hibernate.annotations.ForceDiscriminator` annotation which was deprecated in version 3.6 and later removed. Use `@DiscriminatorOptions` instead.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect5">

    ###### Discriminator formula

    <div class="paragraph">

    Assuming a legacy database schema where the discriminator is based on inspecting a certain column,
    we can take advantage of the Hibernate specific `@DiscriminatorFormula` annotation and map the inheritance model as follows:

    </div>
    <div id="entity-inheritance-single-table-discriminator-formula-example" class="exampleblock">
    <div class="title">Example 185. Single Table discriminator formula</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    @DiscriminatorFormula(
        "case when debitKey is not null " +
        "then 'Debit' " +
        "else ( " +
        "   case when creditKey is not null " +
        "   then 'Credit' " +
        "   else 'Unknown' " +
        "   end ) " +
        "end "
    )
    public static class Account {

        @Id
        private Long id;

        private String owner;

        private BigDecimal balance;

        private BigDecimal interestRate;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getOwner() {
            return owner;
        }

        public void setOwner(String owner) {
            this.owner = owner;
        }

        public BigDecimal getBalance() {
            return balance;
        }

        public void setBalance(BigDecimal balance) {
            this.balance = balance;
        }

        public BigDecimal getInterestRate() {
            return interestRate;
        }

        public void setInterestRate(BigDecimal interestRate) {
            this.interestRate = interestRate;
        }
    }

    @Entity(name = "DebitAccount")
    @DiscriminatorValue(value = "Debit")
    public static class DebitAccount extends Account {

        private String debitKey;

        private BigDecimal overdraftFee;

        private DebitAccount() {
        }

        public DebitAccount(String debitKey) {
            this.debitKey = debitKey;
        }

        public String getDebitKey() {
            return debitKey;
        }

        public BigDecimal getOverdraftFee() {
            return overdraftFee;
        }

        public void setOverdraftFee(BigDecimal overdraftFee) {
            this.overdraftFee = overdraftFee;
        }
    }

    @Entity(name = "CreditAccount")
    @DiscriminatorValue(value = "Credit")
    public static class CreditAccount extends Account {

        private String creditKey;

        private BigDecimal creditLimit;

        private CreditAccount() {
        }

        public CreditAccount(String creditKey) {
            this.creditKey = creditKey;
        }

        public String getCreditKey() {
            return creditKey;
        }

        public BigDecimal getCreditLimit() {
            return creditLimit;
        }

        public void setCreditLimit(BigDecimal creditLimit) {
            this.creditLimit = creditLimit;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Account (
        id int8 NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        debitKey VARCHAR(255) ,
        overdraftFee NUMERIC(19, 2) ,
        creditKey VARCHAR(255) ,
        creditLimit NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `@DiscriminatorFormula` defines a custom SQL clause that can be used to identify a certain subclass type.
    The `@DiscriminatorValue` defines the mapping between the result of the `@DiscriminatorFormula` and the inheritance subclass type.

    </div>
    </div>
    <div class="sect5">

    ###### Implicit discriminator values

    <div class="paragraph">

    Aside from the usual discriminator values assigned to each individual subclass type, the `@DiscriminatorValue` can take two additional values:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`null`</dt>
    <dd>

    If the underlying discriminator column is null, the `null` discriminator mapping is going to be used.

    </dd>
    <dt class="hdlist1">`not null`</dt>
    <dd>

    If the underlying discriminator column has a not-null value that is not explicitly mapped to any entity, the `not-null` discriminator mapping used.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    To understand how these two values work, consider the following entity mapping:

    </div>
    <div id="entity-inheritance-single-table-discriminator-value-example" class="exampleblock">
    <div class="title">Example 186. @DiscriminatorValue `null` and `not-null` entity mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    @DiscriminatorValue( "null" )
    public static class Account {

        @Id
        private Long id;

        private String owner;

        private BigDecimal balance;

        private BigDecimal interestRate;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getOwner() {
            return owner;
        }

        public void setOwner(String owner) {
            this.owner = owner;
        }

        public BigDecimal getBalance() {
            return balance;
        }

        public void setBalance(BigDecimal balance) {
            this.balance = balance;
        }

        public BigDecimal getInterestRate() {
            return interestRate;
        }

        public void setInterestRate(BigDecimal interestRate) {
            this.interestRate = interestRate;
        }
    }

    @Entity(name = "DebitAccount")
    @DiscriminatorValue( "Debit" )
    public static class DebitAccount extends Account {

        private BigDecimal overdraftFee;

        public BigDecimal getOverdraftFee() {
            return overdraftFee;
        }

        public void setOverdraftFee(BigDecimal overdraftFee) {
            this.overdraftFee = overdraftFee;
        }
    }

    @Entity(name = "CreditAccount")
    @DiscriminatorValue( "Credit" )
    public static class CreditAccount extends Account {

        private BigDecimal creditLimit;

        public BigDecimal getCreditLimit() {
            return creditLimit;
        }

        public void setCreditLimit(BigDecimal creditLimit) {
            this.creditLimit = creditLimit;
        }
    }

    @Entity(name = "OtherAccount")
    @DiscriminatorValue( "not null" )
    public static class OtherAccount extends Account {

        private boolean active;

        public boolean isActive() {
            return active;
        }

        public void setActive(boolean active) {
            this.active = active;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `Account` class has a `@DiscriminatorValue( "null" )` mapping, meaning that any `account` row which does not contain any discriminator value will be mapped to an `Account` base class entity.
    The `DebitAccount` and `CreditAccount` entities use explicit discriminator values.
    The `OtherAccount` entity is used as a generic account type because it maps any database row whose discriminator column is not explicitly assigned to any other entity in the current inheritance tree.

    </div>
    <div class="paragraph">

    To visualize how it works, consider the following example:

    </div>
    <div id="entity-inheritance-single-table-discriminator-value-persist-example" class="exampleblock">
    <div class="title">Example 187. @DiscriminatorValue `null` and `not-null` entity persistence</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DebitAccount debitAccount = new DebitAccount();
    debitAccount.setId( 1L );
    debitAccount.setOwner( "John Doe" );
    debitAccount.setBalance( BigDecimal.valueOf( 100 ) );
    debitAccount.setInterestRate( BigDecimal.valueOf( 1.5d ) );
    debitAccount.setOverdraftFee( BigDecimal.valueOf( 25 ) );

    CreditAccount creditAccount = new CreditAccount();
    creditAccount.setId( 2L );
    creditAccount.setOwner( "John Doe" );
    creditAccount.setBalance( BigDecimal.valueOf( 1000 ) );
    creditAccount.setInterestRate( BigDecimal.valueOf( 1.9d ) );
    creditAccount.setCreditLimit( BigDecimal.valueOf( 5000 ) );

    Account account = new Account();
    account.setId( 3L );
    account.setOwner( "John Doe" );
    account.setBalance( BigDecimal.valueOf( 1000 ) );
    account.setInterestRate( BigDecimal.valueOf( 1.9d ) );

    entityManager.persist( debitAccount );
    entityManager.persist( creditAccount );
    entityManager.persist( account );

    entityManager.unwrap( Session.class ).doWork( connection -&gt; {
        try(Statement statement = connection.createStatement()) {
            statement.executeUpdate(
                "insert into Account (DTYPE, active, balance, interestRate, owner, id) " +
                "values ('Other', true, 25, 0.5, 'Vlad', 4)"
            );
        }
    } );

    Map&lt;Long, Account&gt; accounts = entityManager.createQuery(
        "select a from Account a", Account.class )
    .getResultList()
    .stream()
    .collect( Collectors.toMap( Account::getId, Function.identity()));

    assertEquals(4, accounts.size());
    assertEquals( DebitAccount.class, accounts.get( 1L ).getClass() );
    assertEquals( CreditAccount.class, accounts.get( 2L ).getClass() );
    assertEquals( Account.class, accounts.get( 3L ).getClass() );
    assertEquals( OtherAccount.class, accounts.get( 4L ).getClass() );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Account (balance, interestRate, owner, overdraftFee, DTYPE, id)
    VALUES (100, 1.5, 'John Doe', 25, 'Debit', 1)

    INSERT INTO Account (balance, interestRate, owner, overdraftFee, DTYPE, id)
    VALUES (1000, 1.9, 'John Doe', 5000, 'Credit', 2)

    INSERT INTO Account (balance, interestRate, owner, id)
    VALUES (1000, 1.9, 'John Doe', 3)

    INSERT INTO Account (DTYPE, active, balance, interestRate, owner, id)
    VALUES ('Other', true, 25, 0.5, 'Vlad', 4)

    SELECT a.id as id2_0_,
           a.balance as balance3_0_,
           a.interestRate as interest4_0_,
           a.owner as owner5_0_,
           a.overdraftFee as overdraf6_0_,
           a.creditLimit as creditLi7_0_,
           a.active as active8_0_,
           a.DTYPE as DTYPE1_0_
    FROM   Account a`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see, the `Account` entity row has a value of `NULL` in the `DTYPE` discriminator column,
    while the `OtherAccount` entity was saved with a `DTYPE` column value of `other` which has not explicit mapping.

    </div>
    </div>
    </div>
    </div>
    <div class="sect3">
