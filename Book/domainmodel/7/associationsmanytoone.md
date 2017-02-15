 #### 2.7.1. `@ManyToOne`

    <div class="paragraph">

    `@ManyToOne` is the most common association, having a direct equivalent in the relational database as well (e.g. foreign key),
    and so it establishes a relationship between a child entity and a parent.

    </div>
    <div id="associations-many-to-one-example" class="exampleblock">
    <div class="title">Example 125. `@ManyToOne` association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;

        public Person() {
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        @Column(name = "`number`")
        private String number;

        @ManyToOne
        @JoinColumn(name = "person_id",
                foreignKey = @ForeignKey(name = "PERSON_ID_FK")
        )
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
    ADD CONSTRAINT PERSON_ID_FK
    FOREIGN KEY (person_id) REFERENCES Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Each entity has a lifecycle of its own. Once the `@ManyToOne` association is set, Hibernate will set the associated database foreign key column.

    </div>
    <div id="associations-many-to-one-lifecycle-example" class="exampleblock">
    <div class="title">Example 126. `@ManyToOne` association lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    entityManager.persist( person );

    Phone phone = new Phone( "123-456-7890" );
    phone.setPerson( person );
    entityManager.persist( phone );

    entityManager.flush();
    phone.setPerson( null );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person ( id )
    VALUES ( 1 )

    INSERT INTO Phone ( number, person_id, id )
    VALUES ( '123-456-7890', 1, 2 )

    UPDATE Phone
    SET    number = '123-456-7890',
           person_id = NULL
    WHERE  id = 2`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">