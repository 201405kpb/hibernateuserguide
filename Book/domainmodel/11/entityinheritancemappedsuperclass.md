#### 2.11.1. MappedSuperclass

    <div class="paragraph">

    In the following domain model class hierarchy, a 'DebitAccount' and a 'CreditAccount' share the same 'Account' base class.

    </div>
    <div class="paragraph">

    <span class="image">![Inheritance class diagram](images/domain/inheritance/inheritance_class_diagram.svg)</span>

    </div>
    <div class="paragraph">

    When using `MappedSuperclass`, the inheritance is visible in the domain model only and each database table contains both the base class and the subclass properties.

    </div>
    <div id="entity-inheritance-mapped-superclass-example" class="exampleblock">
    <div class="title">Example 181. `@MappedSuperclass` inheritance</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@MappedSuperclass
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
    <pre class="prettyprint highlight">`CREATE TABLE DebitAccount (
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        overdraftFee NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE CreditAccount (
        id BIGINT NOT NULL ,
        balance NUMERIC(19, 2) ,
        interestRate NUMERIC(19, 2) ,
        owner VARCHAR(255) ,
        creditLimit NUMERIC(19, 2) ,
        PRIMARY KEY ( id )
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

    Because the `@MappedSuperclass` inheritance model is not mirrored at database level,
    it&#8217;s not possible to use polymorphic queries (fetching subclasses by their base class).

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">