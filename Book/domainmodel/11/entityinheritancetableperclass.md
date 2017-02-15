 #### 2.11.4. Table per class

    <div class="paragraph">

    A third option is to map only the concrete classes of an inheritance hierarchy to tables.
    This is called the table-per-concrete-class strategy.
    Each table defines all persistent states of the class, including the inherited state.

    </div>
    <div class="paragraph">

    In Hibernate, it is not necessary to explicitly map such inheritance hierarchies.
    You can map each class as a separate entity root.
    However, if you wish use polymorphic associations (e.g. an association to the superclass of your hierarchy), you need to use the union subclass mapping.

    </div>
    <div id="entity-inheritance-table-per-class-example" class="exampleblock">
    <div class="title">Example 191. Table per class</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Account")
    @Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
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
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        creditLimit NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE DebitAccount (
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        overdraftFee NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using polymorphic queries, a UNION is required to fetch the base class table along with all subclass tables as well.

    </div>
    <div class="exampleblock">
    <div class="title">Example 192. Table per class polymorphic query</div>
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
    <pre class="prettyprint highlight">`SELECT tablepercl0_.id AS id1_0_ ,
           tablepercl0_.balance AS balance2_0_ ,
           tablepercl0_.interestRate AS interest3_0_ ,
           tablepercl0_.owner AS owner4_0_ ,
           tablepercl0_.overdraftFee AS overdraf1_2_ ,
           tablepercl0_.creditLimit AS creditLi1_1_ ,
           tablepercl0_.clazz_ AS clazz_
    FROM (
        SELECT    id ,
                 balance ,
                 interestRate ,
                 owner ,
                 CAST(NULL AS INT) AS overdraftFee ,
                 CAST(NULL AS INT) AS creditLimit ,
                 0 AS clazz_
        FROM     Account
        UNION ALL
        SELECT   id ,
                 balance ,
                 interestRate ,
                 owner ,
                 overdraftFee ,
                 CAST(NULL AS INT) AS creditLimit ,
                 1 AS clazz_
        FROM     DebitAccount
        UNION ALL
        SELECT   id ,
                 balance ,
                 interestRate ,
                 owner ,
                 CAST(NULL AS INT) AS overdraftFee ,
                 creditLimit ,
                 2 AS clazz_
        FROM     CreditAccount
    ) tablepercl0_`</pre>
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

    Polymorphic queries require multiple UNION queries, so be aware of the performance implications of a large class hierarchy.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">