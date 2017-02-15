 #### 2.7.2. `@OneToMany`

    <div class="paragraph">

    The `@OneToMany` association links a parent entity with one or more child entities.
    If the `@OneToMany` doesn&#8217;t have a mirroring `@ManyToOne` association on the child side, the `@OneToMany` association is unidirectional.
    If there is a `@ManyToOne` association on the child side, the `@OneToMany` association is bidirectional and the application developer can navigate this relationship from both ends.

    </div>
    <div class="sect4">

    ##### Unidirectional `@OneToMany`

    <div class="paragraph">

    When using a unidirectional `@OneToMany` association, Hibernate resorts to using a link table between the two joining entities.

    </div>
    <div id="associations-one-to-many-unidirectional-example" class="exampleblock">
    <div class="title">Example 127. Unidirectional `@OneToMany` association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;
        @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        public Person() {
        }

        public List&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(String number) {
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getNumber() {
            return number;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Person (
        id BIGINT NOT NULL ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Person_Phone (
        Person_id BIGINT NOT NULL ,
        phones_id BIGINT NOT NULL
    )

    CREATE TABLE Phone (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE Person_Phone
    ADD CONSTRAINT UK_9uhc5itwc9h5gcng944pcaslf
    UNIQUE (phones_id)

    ALTER TABLE Person_Phone
    ADD CONSTRAINT FKr38us2n8g5p9rj0b494sd3391
    FOREIGN KEY (phones_id) REFERENCES Phone

    ALTER TABLE Person_Phone
    ADD CONSTRAINT FK2ex4e4p7w1cj310kg2woisjl2
    FOREIGN KEY (Person_id) REFERENCES Person`</pre>
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

    The `@OneToMany` association is by definition a parent association, even if it&#8217;s a unidirectional or a bidirectional one.
    Only the parent side of an association makes sense to cascade its entity state transitions to children.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="associations-one-to-many-unidirectional-lifecycle-example" class="exampleblock">
    <div class="title">Example 128. Cascading `@OneToMany` association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    Phone phone1 = new Phone( "123-456-7890" );
    Phone phone2 = new Phone( "321-654-0987" );

    person.getPhones().add( phone1 );
    person.getPhones().add( phone2 );
    entityManager.persist( person );
    entityManager.flush();

    person.getPhones().remove( phone1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person
           ( id )
    VALUES ( 1 )

    INSERT INTO Phone
           ( number, id )
    VALUES ( '123 - 456 - 7890', 2 )

    INSERT INTO Phone
           ( number, id )
    VALUES ( '321 - 654 - 0987', 3 )

    INSERT INTO Person_Phone
           ( Person_id, phones_id )
    VALUES ( 1, 2 )

    INSERT INTO Person_Phone
           ( Person_id, phones_id )
    VALUES ( 1, 3 )

    DELETE FROM Person_Phone
    WHERE  Person_id = 1

    INSERT INTO Person_Phone
           ( Person_id, phones_id )
    VALUES ( 1, 3 )

    DELETE FROM Phone
    WHERE  id = 2`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When persisting the `Person` entity, the cascade will propagate the persist operation to the underlying `Phone` children as well.
    Upon removing a `Phone` from the phones collection, the association row is deleted from the link table, and the `orphanRemoval` attribute will trigger a `Phone` removal as well.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The unidirectional associations are not very efficient when it comes to removing child entities.
    In this particular example, upon flushing the persistence context, Hibernate deletes all database child entries and reinserts the ones that are still found in the in-memory persistence context.

    </div>
    <div class="paragraph">

    On the other hand, a bidirectional `@OneToMany` association is much more efficient because the child entity controls the association.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional `@OneToMany`

    <div class="paragraph">

    The bidirectional `@OneToMany` association also requires a `@ManyToOne` association on the child side.
    Although the Domain Model exposes two sides to navigate this association, behind the scenes, the relational database has only one foreign key for this relationship.

    </div>
    <div class="paragraph">

    Every bidirectional association must have one owning side only (the child side), the other one being referred to as the _inverse_ (or the `mappedBy`) side.

    </div>
    <div id="associations-one-to-many-bidirectional-example" class="exampleblock">
    <div class="title">Example 129. `@OneToMany` association mappedBy the `@ManyToOne` side</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;
        @OneToMany(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public List&lt;Phone&gt; getPhones() {
            return phones;
        }

        public void addPhone(Phone phone) {
            phones.add( phone );
            phone.setPerson( this );
        }

        public void removePhone(Phone phone) {
            phones.remove( phone );
            phone.setPerson( null );
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        @NaturalId
        @Column(name = "`number`", unique = true)
        private String number;

        @ManyToOne
        private Person person;

        public Phone() {
        }

        public Phone(String number) {
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getNumber() {
            return number;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Phone phone = (Phone) o;
            return Objects.equals( number, phone.number );
        }

        @Override
        public int hashCode() {
            return Objects.hash( number );
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Person (
        id BIGINT NOT NULL ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE Phone (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        person_id BIGINT ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE Phone
    ADD CONSTRAINT UK_l329ab0g4c1t78onljnxmbnp6
    UNIQUE (number)

    ALTER TABLE Phone
    ADD CONSTRAINT FKmw13yfsjypiiq0i1osdkaeqpg
    FOREIGN KEY (person_id) REFERENCES Person`</pre>
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

    Whenever a bidirectional association is formed, the application developer must make sure both sides are in-sync at all times.
    The `addPhone()` and `removePhone()` are utilities methods that synchronize both ends whenever a child element is added or removed.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Because the `Phone` class has a `@NaturalId` column (the phone number being unique),
    the `equals()` and the `hashCode()` can make use of this property, and so the `removePhone()` logic is reduced to the `remove()` Java `Collection` method.

    </div>
    <div id="associations-one-to-many-bidirectional-lifecycle-example" class="exampleblock">
    <div class="title">Example 130. Bidirectional `@OneToMany` with an owner `@ManyToOne` side lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    Phone phone1 = new Phone( "123-456-7890" );
    Phone phone2 = new Phone( "321-654-0987" );

    person.addPhone( phone1 );
    person.addPhone( phone2 );
    entityManager.persist( person );
    entityManager.flush();

    person.removePhone( phone1 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Phone
           ( number, person_id, id )
    VALUES ( '123-456-7890', NULL, 2 )

    INSERT INTO Phone
           ( number, person_id, id )
    VALUES ( '321-654-0987', NULL, 3 )

    DELETE FROM Phone
    WHERE  id = 2`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Unlike the unidirectional `@OneToMany`, the bidirectional association is much more efficient when managing the collection persistence state.
    Every element removal only requires a single update (in which the foreign key column is set to `NULL`), and,
    if the child entity lifecycle is bound to its owning parent so that the child cannot exist without its parent,
    then we can annotate the association with the `orphan-removal` attribute and disassociating the child will trigger a delete statement on the actual child table row as well.

    </div>
    </div>
    </div>
    <div class="sect3">
