#### 17.10.2. Named SQL queries selecting entities

    <div class="paragraph">

    Considering the following named query:

    </div>
    <div id="sql-entity-NamedNativeQuery-example" class="exampleblock">
    <div class="title">Example 456. Single-entity `NamedNativeQuery`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "find_person_by_name",
        query =
            "SELECT " +
            "   p.id AS \"id\", " +
            "   p.name AS \"name\", " +
            "   p.nickName AS \"nickName\", " +
            "   p.address AS \"address\", " +
            "   p.createdOn AS \"createdOn\", " +
            "   p.version AS \"version\" " +
            "FROM person p " +
            "WHERE p.name LIKE :name",
        resultClass = Person.class
    ),`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The result set mapping declares the entities retrieved by this native query.
    Each field of the entity is bound to an SQL alias (or column name).
    All fields of the entity including the ones of subclasses and the foreign key columns of related entities have to be present in the SQL query.
    Field definitions are optional provided that they map to the same column name as the one declared on the class property.

    </div>
    <div class="paragraph">

    Executing this named native query can be done as follows:

    </div>
    <div id="sql-jpa-entity-named-query-example" class="exampleblock">
    <div class="title">Example 457. JPA named native entity query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createNamedQuery(
        "find_person_by_name" )
    .setParameter("name", "J%")
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-named-query-example" class="exampleblock">
    <div class="title">Example 458. Hibernate named native entity query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.getNamedQuery(
        "find_person_by_name" )
    .setParameter("name", "J%")
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To join multiple entities, you need to use a `SqlResultSetMapping` for each entity the SQL query is going to fetch.

    </div>
    <div id="sql-entity-associations-NamedNativeQuery-example" class="exampleblock">
    <div class="title">Example 459. Joined-entities `NamedNativeQuery`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "find_person_with_phones_by_name",
        query =
            "SELECT " +
            "   pr.id AS \"pr.id\", " +
            "   pr.name AS \"pr.name\", " +
            "   pr.nickName AS \"pr.nickName\", " +
            "   pr.address AS \"pr.address\", " +
            "   pr.createdOn AS \"pr.createdOn\", " +
            "   pr.version AS \"pr.version\", " +
            "   ph.id AS \"ph.id\", " +
            "   ph.person_id AS \"ph.person_id\", " +
            "   ph.phone_number AS \"ph.phone_number\", " +
            "   ph.type AS \"ph.type\" " +
            "FROM person pr " +
            "JOIN phone ph ON pr.id = ph.person_id " +
            "WHERE pr.name LIKE :name",
        resultSetMapping = "person_with_phones"
    )
     @SqlResultSetMapping(
         name = "person_with_phones",
         entities = {
             @EntityResult(
                 entityClass = Person.class,
                 fields = {
                     @FieldResult( name = "id", column = "pr.id" ),
                     @FieldResult( name = "name", column = "pr.name" ),
                     @FieldResult( name = "nickName", column = "pr.nickName" ),
                     @FieldResult( name = "address", column = "pr.address" ),
                     @FieldResult( name = "createdOn", column = "pr.createdOn" ),
                     @FieldResult( name = "version", column = "pr.version" ),
                 }
             ),
             @EntityResult(
                 entityClass = Phone.class,
                 fields = {
                     @FieldResult( name = "id", column = "ph.id" ),
                     @FieldResult( name = "person", column = "ph.person_id" ),
                     @FieldResult( name = "number", column = "ph.number" ),
                     @FieldResult( name = "type", column = "ph.type" ),
                 }
             )
         }
     ),`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-jpa-entity-associations_named-query-example" class="exampleblock">
    <div class="title">Example 460. JPA named native entity query with joined associations</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = entityManager.createNamedQuery(
        "find_person_with_phones_by_name" )
    .setParameter("name", "J%")
    .getResultList();

    for(Object[] tuple : tuples) {
        Person person = (Person) tuple[0];
        Phone phone = (Phone) tuple[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-associations_named-query-example" class="exampleblock">
    <div class="title">Example 461. Hibernate named native entity query with joined associations</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = session.getNamedQuery(
        "find_person_with_phones_by_name" )
    .setParameter("name", "J%")
    .list();

    for(Object[] tuple : tuples) {
        Person person = (Person) tuple[0];
        Phone phone = (Phone) tuple[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Finally, if the association to a related entity involve a composite primary key, a `@FieldResult` element should be used for each foreign key column.
    The `@FieldResult` name is composed of the property name for the relationship, followed by a dot ("."), followed by the name or the field or property of the primary key.
    For this example, the following entities are going to be used:

    </div>
    <div id="sql-composite-key-entity-associations_named-query-example" class="exampleblock">
    <div class="title">Example 462. Entity associations with composite keys and named native queries</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Embeddable
    public class Dimensions {

        private int length;

        private int width;

        public int getLength() {
            return length;
        }

        public void setLength(int length) {
            this.length = length;
        }

        public int getWidth() {
            return width;
        }

        public void setWidth(int width) {
            this.width = width;
        }
    }

    @Embeddable
    public class Identity implements Serializable {

        private String firstname;

        private String lastname;

        public String getFirstname() {
            return firstname;
        }

        public void setFirstname(String firstname) {
            this.firstname = firstname;
        }

        public String getLastname() {
            return lastname;
        }

        public void setLastname(String lastname) {
            this.lastname = lastname;
        }

        public boolean equals(Object o) {
            if ( this == o ) return true;
            if ( o == null || getClass() != o.getClass() ) return false;

            final Identity identity = (Identity) o;

            if ( !firstname.equals( identity.firstname ) ) return false;
            if ( !lastname.equals( identity.lastname ) ) return false;

            return true;
        }

        public int hashCode() {
            int result;
            result = firstname.hashCode();
            result = 29 * result + lastname.hashCode();
            return result;
        }
    }

    @Entity
    public class Captain {

        @EmbeddedId
        private Identity id;

        public Identity getId() {
            return id;
        }

        public void setId(Identity id) {
            this.id = id;
        }
    }

    @Entity
    @NamedNativeQueries({
        @NamedNativeQuery(name = "find_all_spaceships",
            query =
                "SELECT " +
                "   name as \"name\", " +
                "   model, " +
                "   speed, " +
                "   lname as lastn, " +
                "   fname as firstn, " +
                "   length, " +
                "   width, " +
                "   length * width as surface, " +
                "   length * width * 10 as volume " +
                "FROM SpaceShip",
            resultSetMapping = "spaceship"
        )
    })
    @SqlResultSetMapping(
        name = "spaceship",
        entities = @EntityResult(
            entityClass = SpaceShip.class,
            fields = {
                @FieldResult(name = "name", column = "name"),
                @FieldResult(name = "model", column = "model"),
                @FieldResult(name = "speed", column = "speed"),
                @FieldResult(name = "captain.lastname", column = "lastn"),
                @FieldResult(name = "captain.firstname", column = "firstn"),
                @FieldResult(name = "dimensions.length", column = "length"),
                @FieldResult(name = "dimensions.width", column = "width"),
            }
        ),
        columns = {
            @ColumnResult(name = "surface"),
            @ColumnResult(name = "volume")
        }
    )
    public class SpaceShip {

        @Id
        private String name;

        private String model;

        private double speed;

        @ManyToOne(fetch = FetchType.LAZY)
        @JoinColumns({
            @JoinColumn(name = "fname", referencedColumnName = "firstname"),
            @JoinColumn(name = "lname", referencedColumnName = "lastname")
        })
        private Captain captain;

        private Dimensions dimensions;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getModel() {
            return model;
        }

        public void setModel(String model) {
            this.model = model;
        }

        public double getSpeed() {
            return speed;
        }

        public void setSpeed(double speed) {
            this.speed = speed;
        }

        public Captain getCaptain() {
            return captain;
        }

        public void setCaptain(Captain captain) {
            this.captain = captain;
        }

        public Dimensions getDimensions() {
            return dimensions;
        }

        public void setDimensions(Dimensions dimensions) {
            this.dimensions = dimensions;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-jpa-composite-key-entity-associations_named-query-example" class="exampleblock">
    <div class="title">Example 463. JPA named native entity query with joined associations and composite keys</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = entityManager.createNamedQuery(
        "find_all_spaceships" )
    .getResultList();

    for(Object[] tuple : tuples) {
        SpaceShip spaceShip = (SpaceShip) tuple[0];
        Number surface = (Number) tuple[1];
        Number volume = (Number) tuple[2];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-composite-key-entity-associations_named-query-example" class="exampleblock">
    <div class="title">Example 464. Hibernate named native entity query with joined associations and composite keys</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = session.getNamedQuery(
        "find_all_spaceships" )
    .list();

    for(Object[] tuple : tuples) {
        SpaceShip spaceShip = (SpaceShip) tuple[0];
        Number surface = (Number) tuple[1];
        Number volume = (Number) tuple[2];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">