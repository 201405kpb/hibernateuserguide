 #### 6.1.2. `AUTO` flush on JPQL/HQL query

    <div class="paragraph">

    A flush may also be triggered when executing an entity query.

    </div>
    <div id="flushing-auto-flush-jpql-example" class="exampleblock">
    <div class="title">Example 262. Automatic flushing on JPQL/HQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person( "John Doe" );
    entityManager.persist( person );
    entityManager.createQuery( "select p from Advertisement p" ).getResultList();
    entityManager.createQuery( "select p from Person p" ).getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT a.id AS id1_0_ ,
           a.title AS title2_0_
    FROM   Advertisement a

    INSERT INTO Person (name, id) VALUES ('John Doe', 1)

    SELECT p.id AS id1_1_ ,
           p.name AS name2_1_
    FROM   Person p`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The reason why the `Advertisement` entity query didn&#8217;t trigger a flush is because there&#8217;s no overlapping between the `Advertisement` and the `Person` tables:

    </div>
    <div id="flushing-auto-flush-jpql-entity-example" class="exampleblock">
    <div class="title">Example 263. Automatic flushing on JPQL/HQL entities</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        @GeneratedValue
        private Long id;

        private String name;

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

    }

    @Entity(name = "Advertisement")
    public static class Advertisement {

        @Id
        @GeneratedValue
        private Long id;

        private String title;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getTitle() {
            return title;
        }

        public void setTitle(String title) {
            this.title = title;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When querying for a `Person` entity, the flush is triggered prior to executing the entity query.

    </div>
    <div id="flushing-auto-flush-jpql-overlap-example" class="exampleblock">
    <div class="title">Example 264. Automatic flushing on JPQL/HQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person( "John Doe" );
    entityManager.persist( person );
    entityManager.createQuery( "select p from Person p" ).getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person (name, id) VALUES ('John Doe', 1)

    SELECT p.id AS id1_1_ ,
           p.name AS name2_1_
    FROM   Person p`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This time, the flush was triggered by a JPQL query because the pending entity persist action overlaps with the query being executed.

    </div>
    </div>
    <div class="sect3">