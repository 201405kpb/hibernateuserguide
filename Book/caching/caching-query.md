 ### 13.4. Entity cache

    <div id="caching-entity-mapping-example" class="exampleblock">
    <div class="title">Example 311. Entity cache mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Phone")
    @Cacheable
    @org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.NONSTRICT_READ_WRITE)
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        private String mobile;

        @ManyToOne
        private Person person;

        @Version
        private int version;

        public Phone() {}

        public Phone(String mobile) {
            this.mobile = mobile;
        }

        public Long getId() {
            return id;
        }

        public String getMobile() {
            return mobile;
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
    </div>
    </div>
    <div class="paragraph">

    Hibernate stores cached entities in a dehydrated form, which is similar to the database representation.
    Aside from the foreign key column values of the `@ManyToOne` or `@OneToOne` child-side associations,
    entity relationships are not stored in the cache,

    </div>
    <div class="paragraph">

    Once an entity is stored in the second-level cache, you can avoid a database hit and load the entity from the cache alone:

    </div>
    <div id="caching-entity-jpa-example" class="exampleblock">
    <div class="title">Example 312. Loading entity using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-entity-native-example" class="exampleblock">
    <div class="title">Example 313. Loading entity using Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.get( Person.class, 1L );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The Hibernate second-level cache can also load entities by their [natural id](#naturalid):

    </div>
    <div id="caching-entity-natural-id-mapping-example" class="exampleblock">
    <div class="title">Example 314. Hibernate natural id entity mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    @Cacheable
    @org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
    public static class Person {

        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        private Long id;

        private String name;

        @NaturalId
        @Column(name = "code", unique = true)
        private String code;

        public Person() {}

        public Person(String name) {
            this.name = name;
        }

        public Long getId() {
            return id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getCode() {
            return code;
        }

        public void setCode(String code) {
            this.code = code;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-entity-natural-id-example" class="exampleblock">
    <div class="title">Example 315. Loading entity using Hibernate native natural id API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session
        .byNaturalId( Person.class )
        .using( "code", "unique-code")
        .load();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">