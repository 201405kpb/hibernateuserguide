 #### 2.8.8. Maps

    <div class="paragraph">

    A `java.util.Map` is ternary association because it required a parent entity a map key and a value.
    An entity can either be a map key or a map value, depending on the mapping.
    Hibernate allows using the following map keys:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`MapKeyColumn`</dt>
    <dd>

    for value type maps, the map key is a column in the link table that defines the grouping logic

    </dd>
    <dt class="hdlist1">`MapKey`</dt>
    <dd>

    the map key is either the primary key or another property of the entity stored as a map entry value

    </dd>
    <dt class="hdlist1">`MapKeyEnumerated`</dt>
    <dd>

    the map key is an `Enum` of the target child entity

    </dd>
    <dt class="hdlist1">`MapKeyTemporal`</dt>
    <dd>

    the map key is a `Date` or a `Calendar` of the target child entity

    </dd>
    <dt class="hdlist1">`MapKeyJoinColumn`</dt>
    <dd>

    the map key is an entity mapped as an association in the child entity that&#8217;s stored as a map entry key

    </dd>
    </dl>
    </div>
    <div class="sect4">

    ##### Value type maps

    <div class="paragraph">

    A map of value type must use the `@ElementCollection` annotation, just like value type lists, bags or sets.

    </div>
    <div id="collections-map-value-type-entity-key-example" class="exampleblock">
    <div class="title">Example 163. Value type map with an entity as a map key</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public enum PhoneType {
        LAND_LINE,
        MOBILE
    }

    @Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @Temporal(TemporalType.TIMESTAMP)
        @ElementCollection
        @CollectionTable(name = "phone_register")
        @Column(name = "since")
        @MapKeyJoinColumn(name = "phone_id", referencedColumnName = "id")
        private Map&lt;Phone, Date&gt; phoneRegister = new HashMap&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public Map&lt;Phone, Date&gt; getPhoneRegister() {
            return phoneRegister;
        }
    }

    @Embeddable
    public static class Phone {

        private PhoneType type;

        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(PhoneType type, String number) {
            this.type = type;
            this.number = number;
        }

        public PhoneType getType() {
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

    CREATE TABLE phone_register (
        Person_id BIGINT NOT NULL ,
        since TIMESTAMP ,
        number VARCHAR(255) NOT NULL ,
        type INTEGER NOT NULL ,
        PRIMARY KEY ( Person_id, number, type )
    )

    ALTER TABLE phone_register
    ADD CONSTRAINT FKrmcsa34hr68of2rq8qf526mlk
    FOREIGN KEY (Person_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Adding entries to the map generates the following SQL statements:

    </div>
    <div id="collections-map-value-type-entity-key-add-example" class="exampleblock">
    <div class="title">Example 164. Adding value type map entries</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`person.getPhoneRegister().put(
        new Phone( PhoneType.LAND_LINE, "028-234-9876" ), new Date()
    );
    person.getPhoneRegister().put(
        new Phone( PhoneType.MOBILE, "072-122-9876" ), new Date()
    );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO phone_register (Person_id, number, type, since)
    VALUES (1, '072-122-9876', 1, '2015-12-15 17:16:45.311')

    INSERT INTO phone_register (Person_id, number, type, since)
    VALUES (1, '028-234-9876', 0, '2015-12-15 17:16:45.311')`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Unidirectional maps

    <div class="paragraph">

    A unidirectional map exposes a parent-child association from the parent-side only.
    The following example shows a unidirectional map which also uses a `@MapKeyTemporal` annotation.
    The map key is a timestamp and it&#8217;s taken from the child entity table.

    </div>
    <div id="collections-map-unidirectional-example" class="exampleblock">
    <div class="title">Example 165. Unidirectional Map</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public enum PhoneType {
        LAND_LINE,
        MOBILE
    }

    @Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
        @JoinTable(
                name = "phone_register",
                joinColumns = @JoinColumn(name = "phone_id"),
                inverseJoinColumns = @JoinColumn(name = "person_id"))
        @MapKey(name = "since")
        @MapKeyTemporal(TemporalType.TIMESTAMP)
        private Map&lt;Date, Phone&gt; phoneRegister = new HashMap&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public Map&lt;Date, Phone&gt; getPhoneRegister() {
            return phoneRegister;
        }

        public void addPhone(Phone phone) {
            phoneRegister.put( phone.getSince(), phone );
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        private PhoneType type;

        @Column(name = "`number`")
        private String number;

        private Date since;

        public Phone() {
        }

        public Phone(PhoneType type, String number, Date since) {
            this.type = type;
            this.number = number;
            this.since = since;
        }

        public PhoneType getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }

        public Date getSince() {
            return since;
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
        since TIMESTAMP ,
        type INTEGER ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE phone_register (
        phone_id BIGINT NOT NULL ,
        person_id BIGINT NOT NULL ,
        PRIMARY KEY ( phone_id, person_id )
    )

    ALTER TABLE phone_register
    ADD CONSTRAINT FKc3jajlx41lw6clbygbw8wm65w
    FOREIGN KEY (person_id) REFERENCES Phone

    ALTER TABLE phone_register
    ADD CONSTRAINT FK6npoomh1rp660o1b55py9ndw4
    FOREIGN KEY (phone_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional maps

    <div class="paragraph">

    Like most bidirectional associations, this relationship is owned by the child-side while the parent is the inverse side abd can propagate its own state transitions to the child entities.
    In the following example, you can see that `@MapKeyEnumerated` was used so that the `Phone` enumeration becomes the map key.

    </div>
    <div id="collections-map-bidirectional-example" class="exampleblock">
    <div class="title">Example 166. Bidirectional Map</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
        @MapKey(name = "type")
        @MapKeyEnumerated
        private Map&lt;PhoneType, Phone&gt; phoneRegister = new HashMap&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public Map&lt;PhoneType, Phone&gt; getPhoneRegister() {
            return phoneRegister;
        }

        public void addPhone(Phone phone) {
            phone.setPerson( this );
            phoneRegister.put( phone.getType(), phone );
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        private PhoneType type;

        @Column(name = "`number`")
        private String number;

        private Date since;

        @ManyToOne
        private Person person;

        public Phone() {
        }

        public Phone(PhoneType type, String number, Date since) {
            this.type = type;
            this.number = number;
            this.since = since;
        }

        public PhoneType getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }

        public Date getSince() {
            return since;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
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
        since TIMESTAMP ,
        type INTEGER ,
        person_id BIGINT ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE Phone
    ADD CONSTRAINT FKmw13yfsjypiiq0i1osdkaeqpg
    FOREIGN KEY (person_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">