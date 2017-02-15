 #### 2.7.4. `@ManyToMany`

    <div class="paragraph">

    The `@ManyToMany` association requires a link table that joins two entities.
    Like the `@OneToMany` association, `@ManyToMany` can be a either unidirectional or bidirectional.

    </div>
    <div class="sect4">

    ##### Unidirectional `@ManyToMany`

    <div id="associations-many-to-many-unidirectional-example" class="exampleblock">
    <div class="title">Example 135. Unidirectional `@ManyToMany`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;
        @ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
        private List&lt;Address&gt; addresses = new ArrayList&lt;&gt;();

        public Person() {
        }

        public List&lt;Address&gt; getAddresses() {
            return addresses;
        }
    }

    @Entity(name = "Address")
    public static class Address {

        @Id
        @GeneratedValue
        private Long id;

        private String street;

        @Column(name = "`number`")
        private String number;

        public Address() {
        }

        public Address(String street, String number) {
            this.street = street;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getStreet() {
            return street;
        }

        public String getNumber() {
            return number;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Address (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        street VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person (
        id BIGINT NOT NULL ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person_Address (
        Person_id BIGINT NOT NULL ,
        addresses_id BIGINT NOT NULL
    )

    ALTER TABLE Person_Address
    ADD CONSTRAINT FKm7j0bnabh2yr0pe99il1d066u
    FOREIGN KEY (addresses_id) REFERENCES Address

    ALTER TABLE Person_Address
    ADD CONSTRAINT FKba7rc9qe2vh44u93u0p2auwti
    FOREIGN KEY (Person_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Just like with unidirectional `@OneToMany` associations, the link table is controlled by the owning side.

    </div>
    <div class="paragraph">

    When an entity is removed from the `@ManyToMany` collection, Hibernate simply deletes the joining record in the link table.
    Unfortunately, this operation requires removing all entries associated with a given parent and recreating the ones that are listed in the current running persistent context.

    </div>
    <div id="associations-many-to-many-unidirectional-lifecycle-example" class="exampleblock">
    <div class="title">Example 136. Unidirectional `@ManyToMany` lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person1 = new Person();
    Person person2 = new Person();

    Address address1 = new Address( "12th Avenue", "12A" );
    Address address2 = new Address( "18th Avenue", "18B" );

    person1.getAddresses().add( address1 );
    person1.getAddresses().add( address2 );

    person2.getAddresses().add( address1 );

    entityManager.persist( person1 );
    entityManager.persist( person2 );

    entityManager.flush();

    person1.getAddresses().remove( address1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person ( id )
    VALUES ( 1 )

    INSERT INTO Address ( number, street, id )
    VALUES ( '12A', '12th Avenue', 2 )

    INSERT INTO Address ( number, street, id )
    VALUES ( '18B', '18th Avenue', 3 )

    INSERT INTO Person ( id )
    VALUES ( 4 )

    INSERT INTO Person_Address ( Person_id, addresses_id )
    VALUES ( 1, 2 )
    INSERT INTO Person_Address ( Person_id, addresses_id )
    VALUES ( 1, 3 )
    INSERT INTO Person_Address ( Person_id, addresses_id )
    VALUES ( 4, 2 )

    DELETE FROM Person_Address
    WHERE  Person_id = 1

    INSERT INTO Person_Address ( Person_id, addresses_id )
    VALUES ( 1, 3 )`</pre>
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

    For `@ManyToMany` associations, the `REMOVE` entity state transition doesn&#8217;t make sense to be cascaded because it will propagate beyond the link table.
    Since the other side might be referenced by other entities on the parent-side, the automatic removal might end up in a `ConstraintViolationException`.

    </div>
    <div class="paragraph">

    For example, if `@ManyToMany(cascade = CascadeType.ALL)` was defined and the first person would be deleted,
    Hibernate would throw an exception because another person is still associated with the address that&#8217;s being deleted.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person1 = entityManager.find(Person.class, personId);
    entityManager.remove(person1);

    Caused by: javax.persistence.PersistenceException: org.hibernate.exception.ConstraintViolationException: could not execute statement
    Caused by: org.hibernate.exception.ConstraintViolationException: could not execute statement
    Caused by: java.sql.SQLIntegrityConstraintViolationException: integrity constraint violation: foreign key no action; FKM7J0BNABH2YR0PE99IL1D066U table: PERSON_ADDRESS`</pre>
    </div>
    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    By simply removing the parent-side, Hibernate can safely remove the associated link records as you can see in the following example:

    </div>
    <div id="associations-many-to-many-unidirectional-remove-example" class="exampleblock">
    <div class="title">Example 137. Unidirectional `@ManyToMany` entity removal</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person1 = entityManager.find( Person.class, personId );
    entityManager.remove( person1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DELETE FROM Person_Address
    WHERE  Person_id = 1

    DELETE FROM Person
    WHERE  id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional `@ManyToMany`

    <div class="paragraph">

    A bidirectional `@ManyToMany` association has an owning and a `mappedBy` side.
    To preserve synchronicity between both sides, it&#8217;s good practice to provide helper methods for adding or removing child entities.

    </div>
    <div id="associations-many-to-many-bidirectional-example" class="exampleblock">
    <div class="title">Example 138. Bidirectional `@ManyToMany`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;

        @NaturalId
        private String registrationNumber;
        @ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
        private List&lt;Address&gt; addresses = new ArrayList&lt;&gt;();

        public Person() {
        }

        public Person(String registrationNumber) {
            this.registrationNumber = registrationNumber;
        }

        public List&lt;Address&gt; getAddresses() {
            return addresses;
        }

        public void addAddress(Address address) {
            addresses.add( address );
            address.getOwners().add( this );
        }

        public void removeAddress(Address address) {
            addresses.remove( address );
            address.getOwners().remove( this );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Person person = (Person) o;
            return Objects.equals( registrationNumber, person.registrationNumber );
        }

        @Override
        public int hashCode() {
            return Objects.hash( registrationNumber );
        }
    }

    @Entity(name = "Address")
    public static class Address {

        @Id
        @GeneratedValue
        private Long id;

        private String street;

        @Column(name = "`number`")
        private String number;

        private String postalCode;

        @ManyToMany(mappedBy = "addresses")
        private List&lt;Person&gt; owners = new ArrayList&lt;&gt;();

        public Address() {
        }

        public Address(String street, String number, String postalCode) {
            this.street = street;
            this.number = number;
            this.postalCode = postalCode;
        }

        public Long getId() {
            return id;
        }

        public String getStreet() {
            return street;
        }

        public String getNumber() {
            return number;
        }

        public String getPostalCode() {
            return postalCode;
        }

        public List&lt;Person&gt; getOwners() {
            return owners;
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Address address = (Address) o;
            return Objects.equals( street, address.street ) &amp;&amp;
                    Objects.equals( number, address.number ) &amp;&amp;
                    Objects.equals( postalCode, address.postalCode );
        }

        @Override
        public int hashCode() {
            return Objects.hash( street, number, postalCode );
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Address (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        postalCode VARCHAR(255) ,
        street VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person (
        id BIGINT NOT NULL ,
        registrationNumber VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person_Address (
        owners_id BIGINT NOT NULL ,
        addresses_id BIGINT NOT NULL
    )

    ALTER TABLE Person
    ADD CONSTRAINT UK_23enodonj49jm8uwec4i7y37f
    UNIQUE (registrationNumber)

    ALTER TABLE Person_Address
    ADD CONSTRAINT FKm7j0bnabh2yr0pe99il1d066u
    FOREIGN KEY (addresses_id) REFERENCES Address

    ALTER TABLE Person_Address
    ADD CONSTRAINT FKbn86l24gmxdv2vmekayqcsgup
    FOREIGN KEY (owners_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    With the helper methods in place, the synchronicity management can be simplified, as you can see in the following example:

    </div>
    <div id="associations-many-to-many-bidirectional-lifecycle-example" class="exampleblock">
    <div class="title">Example 139. Bidirectional `@ManyToMany` lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person1 = new Person( "ABC-123" );
    Person person2 = new Person( "DEF-456" );

    Address address1 = new Address( "12th Avenue", "12A", "4005A" );
    Address address2 = new Address( "18th Avenue", "18B", "4007B" );

    person1.addAddress( address1 );
    person1.addAddress( address2 );

    person2.addAddress( address1 );

    entityManager.persist( person1 );
    entityManager.persist( person2 );

    entityManager.flush();

    person1.removeAddress( address1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person ( registrationNumber, id )
    VALUES ( 'ABC-123', 1 )

    INSERT INTO Address ( number, postalCode, street, id )
    VALUES ( '12A', '4005A', '12th Avenue', 2 )

    INSERT INTO Address ( number, postalCode, street, id )
    VALUES ( '18B', '4007B', '18th Avenue', 3 )

    INSERT INTO Person ( registrationNumber, id )
    VALUES ( 'DEF-456', 4 )

    INSERT INTO Person_Address ( owners_id, addresses_id )
    VALUES ( 1, 2 )

    INSERT INTO Person_Address ( owners_id, addresses_id )
    VALUES ( 1, 3 )

    INSERT INTO Person_Address ( owners_id, addresses_id )
    VALUES ( 4, 2 )

    DELETE FROM Person_Address
    WHERE  owners_id = 1

    INSERT INTO Person_Address ( owners_id, addresses_id )
    VALUES ( 1, 3 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If a bidirectional `@OneToMany` association performs better when removing or changing the order of child elements,
    the `@ManyToMany` relationship cannot benefit from such an optimization because the foreign key side is not in control.
    To overcome this limitation, the link table must be directly exposed and the `@ManyToMany` association split into two bidirectional `@OneToMany` relationships.

    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional many-to-many with a link entity

    <div class="paragraph">

    To most natural `@ManyToMany` association follows the same logic employed by the database schema,
    and the link table has an associated entity which controls the relationship for both sides that need to be joined.

    </div>
    <div id="associations-many-to-many-bidirectional-with-link-entity-example" class="exampleblock">
    <div class="title">Example 140. Bidirectional many-to-many with link entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person implements Serializable {

        @Id
        @GeneratedValue
        private Long id;

        @NaturalId
        private String registrationNumber;

        @OneToMany(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
        private List&lt;PersonAddress&gt; addresses = new ArrayList&lt;&gt;();

        public Person() {
        }

        public Person(String registrationNumber) {
            this.registrationNumber = registrationNumber;
        }

        public Long getId() {
            return id;
        }

        public List&lt;PersonAddress&gt; getAddresses() {
            return addresses;
        }

        public void addAddress(Address address) {
            PersonAddress personAddress = new PersonAddress( this, address );
            addresses.add( personAddress );
            address.getOwners().add( personAddress );
        }

        public void removeAddress(Address address) {
            PersonAddress personAddress = new PersonAddress( this, address );
            address.getOwners().remove( personAddress );
            addresses.remove( personAddress );
            personAddress.setPerson( null );
            personAddress.setAddress( null );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Person person = (Person) o;
            return Objects.equals( registrationNumber, person.registrationNumber );
        }

        @Override
        public int hashCode() {
            return Objects.hash( registrationNumber );
        }
    }

    @Entity(name = "PersonAddress")
    public static class PersonAddress implements Serializable {

        @Id
        @ManyToOne
        private Person person;

        @Id
        @ManyToOne
        private Address address;

        public PersonAddress() {
        }

        public PersonAddress(Person person, Address address) {
            this.person = person;
            this.address = address;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
        }

        public Address getAddress() {
            return address;
        }

        public void setAddress(Address address) {
            this.address = address;
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            PersonAddress that = (PersonAddress) o;
            return Objects.equals( person, that.person ) &amp;&amp;
                    Objects.equals( address, that.address );
        }

        @Override
        public int hashCode() {
            return Objects.hash( person, address );
        }
    }

    @Entity(name = "Address")
    public static class Address implements Serializable {

        @Id
        @GeneratedValue
        private Long id;

        private String street;

        @Column(name = "`number`")
        private String number;

        private String postalCode;

        @OneToMany(mappedBy = "address", cascade = CascadeType.ALL, orphanRemoval = true)
        private List&lt;PersonAddress&gt; owners = new ArrayList&lt;&gt;();

        public Address() {
        }

        public Address(String street, String number, String postalCode) {
            this.street = street;
            this.number = number;
            this.postalCode = postalCode;
        }

        public Long getId() {
            return id;
        }

        public String getStreet() {
            return street;
        }

        public String getNumber() {
            return number;
        }

        public String getPostalCode() {
            return postalCode;
        }

        public List&lt;PersonAddress&gt; getOwners() {
            return owners;
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Address address = (Address) o;
            return Objects.equals( street, address.street ) &amp;&amp;
                    Objects.equals( number, address.number ) &amp;&amp;
                    Objects.equals( postalCode, address.postalCode );
        }

        @Override
        public int hashCode() {
            return Objects.hash( street, number, postalCode );
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Address (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        postalCode VARCHAR(255) ,
        street VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person (
        id BIGINT NOT NULL ,
        registrationNumber VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE PersonAddress (
        person_id BIGINT NOT NULL ,
        address_id BIGINT NOT NULL ,
        PRIMARY KEY ( person_id, address_id )
    )

    ALTER TABLE Person
    ADD CONSTRAINT UK_23enodonj49jm8uwec4i7y37f
    UNIQUE (registrationNumber)

    ALTER TABLE PersonAddress
    ADD CONSTRAINT FK8b3lru5fyej1aarjflamwghqq
    FOREIGN KEY (person_id) REFERENCES Person

    ALTER TABLE PersonAddress
    ADD CONSTRAINT FK7p69mgialumhegyl4byrh65jk
    FOREIGN KEY (address_id) REFERENCES Address`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Both the `Person` and the `Address` have a` mappedBy` `@OneToMany` side, while the `PersonAddress` owns the `person` and the `address` `@ManyToOne` associations.
    Because this mapping is formed out of two bidirectional associations, the helper methods are even more relevant.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The aforementioned example uses a Hibernate specific mapping for the link entity since JPA doesn&#8217;t allow building a composite identifier out of multiple `@ManyToOne` associations.
    For more details, see the [Composite identifiers - associations](#identifiers-composite-associations) section.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The entity state transitions are better managed than in the previous bidirectional `@ManyToMany` case.

    </div>
    <div id="associations-many-to-many-bidirectional-with-link-entity-lifecycle-example" class="exampleblock">
    <div class="title">Example 141. Bidirectional many-to-many with link entity lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person1 = new Person( "ABC-123" );
    Person person2 = new Person( "DEF-456" );

    Address address1 = new Address( "12th Avenue", "12A", "4005A" );
    Address address2 = new Address( "18th Avenue", "18B", "4007B" );

    entityManager.persist( person1 );
    entityManager.persist( person2 );

    entityManager.persist( address1 );
    entityManager.persist( address2 );

    person1.addAddress( address1 );
    person1.addAddress( address2 );

    person2.addAddress( address1 );

    entityManager.flush();

    log.info( "Removing address" );
    person1.removeAddress( address1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT  INTO Person ( registrationNumber, id )
    VALUES  ( 'ABC-123', 1 )

    INSERT  INTO Person ( registrationNumber, id )
    VALUES  ( 'DEF-456', 2 )

    INSERT  INTO Address ( number, postalCode, street, id )
    VALUES  ( '12A', '4005A', '12th Avenue', 3 )

    INSERT  INTO Address ( number, postalCode, street, id )
    VALUES  ( '18B', '4007B', '18th Avenue', 4 )

    INSERT  INTO PersonAddress ( person_id, address_id )
    VALUES  ( 1, 3 )

    INSERT  INTO PersonAddress ( person_id, address_id )
    VALUES  ( 1, 4 )

    INSERT  INTO PersonAddress ( person_id, address_id )
    VALUES  ( 2, 3 )

    DELETE  FROM PersonAddress
    WHERE   person_id = 1 AND address_id = 3`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    There is only one delete statement executed because, this time, the association is controlled by the `@ManyToOne` side which only has to monitor the state of the underlying foreign key relationship to trigger the right DML statement.

    </div>
    </div>
    </div>
    </div>
    <div class="sect2">