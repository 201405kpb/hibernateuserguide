 #### 2.11.3. Joined table

    <div class="paragraph">

    Each subclass can also be mapped to its own table.
    This is also called _table-per-subclass_ mapping strategy.
    An inherited state is retrieved by joining with the table of the superclass.

    </div>
    <div class="paragraph">

    A discriminator column is not required for this mapping strategy.
    Each subclass must, however, declare a table column holding the object identifier.

    </div>
    <div id="entity-inheritance-joined-table-example" class="exampleblock">
    <div class="title">Example 188. Join Table</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.JOINED)
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
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE CreditAccount (
        creditLimit NUMERIC(19, 2) ,
        id BIGINT NOT NULL ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE DebitAccount (
        overdraftFee NUMERIC(19, 2) ,
        id BIGINT NOT NULL ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE CreditAccount
    ADD CONSTRAINT FKihw8h3j1k0w31cnyu7jcl7n7n
    FOREIGN KEY (id) REFERENCES Account

    ALTER TABLE DebitAccount
    ADD CONSTRAINT FKia914478noepymc468kiaivqm
    FOREIGN KEY (id) REFERENCES Account`</pre>
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

    The primary key of this table is also a foreign key to the superclass table and described by the `@PrimaryKeyJoinColumns`.

    </div>
    <div class="paragraph">

    The table name still defaults to the non-qualified class name.
    Also, if `@PrimaryKeyJoinColumn` is not set, the primary key / foreign key columns are assumed to have the same names as the primary key columns of the primary table of the superclass.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="entity-inheritance-joined-table-primary-key-join-column-example" class="exampleblock">
    <div class="title">Example 189. Join Table with `@PrimaryKeyJoinColumn`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.JOINED)
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
    @PrimaryKeyJoinColumn(name = "account_id")
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
    @PrimaryKeyJoinColumn(name = "account_id")
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
    <pre class="prettyprint highlight">`CREATE TABLE CreditAccount (
        creditLimit NUMERIC(19, 2) ,
        account_id BIGINT NOT NULL ,
        PRIMARY KEY ( account_id )
    )

    CREATE TABLE DebitAccount (
        overdraftFee NUMERIC(19, 2) ,
        account_id BIGINT NOT NULL ,
        PRIMARY KEY ( account_id )
    )

    ALTER TABLE CreditAccount
    ADD CONSTRAINT FK8ulmk1wgs5x7igo370jt0q005
    FOREIGN KEY (account_id) REFERENCES Account

    ALTER TABLE DebitAccount
    ADD CONSTRAINT FK7wjufa570onoidv4omkkru06j
    FOREIGN KEY (account_id) REFERENCES Account`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using polymorphic queries, the base class table must be joined with all subclass tables to fetch every associated subclass instance.

    </div>
    <div id="entity-inheritance-joined-table-query-example" class="exampleblock">
    <div class="title">Example 190. Join Table polymorphic query</div>
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
    <pre class="prettyprint highlight">`SELECT jointablet0_.id AS id1_0_ ,
           jointablet0_.balance AS balance2_0_ ,
           jointablet0_.interestRate AS interest3_0_ ,
           jointablet0_.owner AS owner4_0_ ,
           jointablet0_1_.overdraftFee AS overdraf1_2_ ,
           jointablet0_2_.creditLimit AS creditLi1_1_ ,
           CASE WHEN jointablet0_1_.id IS NOT NULL THEN 1
                WHEN jointablet0_2_.id IS NOT NULL THEN 2
                WHEN jointablet0_.id IS NOT NULL THEN 0
           END AS clazz_
    FROM   Account jointablet0_
           LEFT OUTER JOIN DebitAccount jointablet0_1_ ON jointablet0_.id = jointablet0_1_.id
           LEFT OUTER JOIN CreditAccount jointablet0_2_ ON jointablet0_.id = jointablet0_2_.id`</pre>
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

    The joined table inheritance polymorphic queries can use several JOINS which might affect performance when fetching a large number of entities.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">