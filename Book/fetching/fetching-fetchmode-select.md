 ### 11.9. `FetchMode.SELECT`

    <div class="paragraph">

    To demonstrate how `FetchMode.SELECT` works, consider the following entity mapping:

    </div>
    <div id="fetching-strategies-fetch-mode-select-mapping-example" class="exampleblock">
    <div class="title">Example 292. `FetchMode.SELECT` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Department")
    public static class Department {

        @Id
        private Long id;

        @OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
        @Fetch(FetchMode.SELECT)
        private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity

    }

    @Entity(name = "Employee")
    public static class Employee {

        @Id
        @GeneratedValue
        private Long id;

        @NaturalId
        private String username;

        @ManyToOne(fetch = FetchType.LAZY)
        private Department department;

        //Getters and setters omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Considering there are multiple `Department` entities, each one having multiple `Employee` entities,
    when executing the following test case, Hibernate fetches every uninitialized `Employee`
    collection using a secondary `SELECT` statement upon accessing the child collection for the first time:

    </div>
    <div id="fetching-strategies-fetch-mode-select-example" class="exampleblock">
    <div class="title">Example 293. `FetchMode.SELECT` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Department&gt; departments = entityManager.createQuery(
        "select d from Department d", Department.class )
    .getResultList();

    log.infof( "Fetched %d Departments", departments.size());

    for (Department department : departments ) {
        assertEquals( 3, department.getEmployees().size() );
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        d.id as id1_0_
    FROM
        Department d

    -- Fetched 2 Departments

    SELECT
        e.department_id as departme3_1_0_,
        e.id as id1_1_0_,
        e.id as id1_1_1_,
        e.department_id as departme3_1_1_,
        e.username as username2_1_1_
    FROM
        Employee e
    WHERE
        e.department_id = 1

    SELECT
        e.department_id as departme3_1_0_,
        e.id as id1_1_0_,
        e.id as id1_1_1_,
        e.department_id as departme3_1_1_,
        e.username as username2_1_1_
    FROM
        Employee e
    WHERE
        e.department_id = 2`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The more `Department` entities are fetched by the first query, the more secondary `SELECT` statements are executed to initialize the `employees` collections.
    Therefore, `FetchMode.SELECT` can lead to N+1 query issues.

    </div>
    </div>
    <div class="sect2">