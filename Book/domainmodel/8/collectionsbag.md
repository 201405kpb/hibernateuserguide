 #### 2.8.4. Bags

    <div class="paragraph">

    Bags are unordered lists and we can have unidirectional bags or bidirectional ones.

    </div>
    <div class="sect4">

    ##### Unidirectional bags

    <div class="paragraph">

    The unidirectional bag is mapped using a single `@OneToMany` annotation on the parent side of the association.
    Behind the scenes, Hibernate requires an association table to manage the parent-child relationship, as we can see in the following example:

    </div>
    <div id="collections-unidirectional-bag-example" class="exampleblock">
    <div class="title">Example 147. Unidirectional bag</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(cascade = CascadeType.ALL)
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public List&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        private Long id;

        private String type;

        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(Long id, String type, String number) {
            this.id = id;
            this.type = type;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getType() {
            return type;
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
        type VARCHAR(255) ,
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

    Because both the parent and the child sides are entities, the persistence context manages each entity separately.
    Cascades can propagate an entity state transition from a parent entity to its children.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    By marking the parent side with the `CascadeType.ALL` attribute, the unidirectional association lifecycle becomes very similar to that of a value type collection.

    </div>
    <div id="collections-unidirectional-bag-lifecycle-example" class="exampleblock">
    <div class="title">Example 148. Unidirectional bag lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person( 1L );
    person.getPhones().add( new Phone( 1L, "landline", "028-234-9876" ) );
    person.getPhones().add( new Phone( 2L, "mobile", "072-122-9876" ) );
    entityManager.persist( person );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person ( id )
    VALUES ( 1 )

    INSERT INTO Phone ( number, type, id )
    VALUES ( '028-234-9876', 'landline', 1 )

    INSERT INTO Phone ( number, type, id )
    VALUES ( '072-122-9876', 'mobile', 2 )

    INSERT INTO Person_Phone ( Person_id, phones_id )
    VALUES ( 1, 1 )

    INSERT INTO Person_Phone ( Person_id, phones_id )
    VALUES ( 1, 2 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the example above, once the parent entity is persisted, the child entities are going to be persisted as well.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Just like value type collections, unidirectional bags are not as efficient when it comes to modifying the collection structure (removing or reshuffling elements).
    Because the parent-side cannot uniquely identify each individual child, Hibernate might delete all child table rows associated with the parent entity and re-add them according to the current collection state.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional bags

    <div class="paragraph">

    The bidirectional bag is the most common type of entity collection.
    The `@ManyToOne` side is the owning side of the bidirectional bag association, while the `@OneToMany` is the _inverse_ side, being marked with the `mappedBy` attribute.

    </div>
    <div id="collections-bidirectional-bag-example" class="exampleblock">
    <div class="title">Example 149. Bidirectional bag</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
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
        private Long id;

        private String type;

        @Column(name = "`number`", unique = true)
        @NaturalId
        private String number;

        @ManyToOne
        private Person person;

        public Phone() {
        }

        public Phone(Long id, String type, String number) {
            this.id = id;
            this.type = type;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getType() {
            return type;
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
        id BIGINT NOT NULL, PRIMARY KEY (id)
    )

    CREATE TABLE Phone (
        id BIGINT NOT NULL,
        number VARCHAR(255),
        type VARCHAR(255),
        person_id BIGINT,
        PRIMARY KEY (id)
    )

    ALTER TABLE Phone
    ADD CONSTRAINT UK_l329ab0g4c1t78onljnxmbnp6
    UNIQUE (number)

    ALTER TABLE Phone
    ADD CONSTRAINT FKmw13yfsjypiiq0i1osdkaeqpg
    FOREIGN KEy (person_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="collections-bidirectional-bag-lifecycle-example" class="exampleblock">
    <div class="title">Example 150. Bidirectional bag lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`person.addPhone( new Phone( 1L, "landline", "028-234-9876" ) );
    person.addPhone( new Phone( 2L, "mobile", "072-122-9876" ) );
    entityManager.flush();
    person.removePhone( person.getPhones().get( 0 ) );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Phone (number, person_id, type, id)
    VALUES ( '028-234-9876', 1, 'landline', 1 )

    INSERT INTO Phone (number, person_id, type, id)
    VALUES ( '072-122-9876', 1, 'mobile', 2 )

    UPDATE Phone
    SET person_id = NULL, type = 'landline' where id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="collections-bidirectional-bag-orphan-removal-example" class="exampleblock">
    <div class="title">Example 151. Bidirectional bag with orphan removal</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
    private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DELETE FROM Phone WHERE id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When rerunning the previous example, the child will get removed because the parent-side propagates the removal upon disassociating the child entity reference.

    </div>
    </div>
    </div>
    <div class="sect3">
