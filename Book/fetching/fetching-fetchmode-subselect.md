### 11.10. `FetchMode.SUBSELECT`

    <div class="paragraph">

    To demonstrate how `FetchMode.SUBSELECT` works, we are going to modify the [`FetchMode.SELECT` mapping example](#fetching-strategies-fetch-mode-select-mapping-example) to use
    `FetchMode.SUBSELECT`:

    </div>
    <div id="fetching-strategies-fetch-mode-subselect-mapping-example" class="exampleblock">
    <div class="title">Example 294. `FetchMode.SUBSELECT` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
    @Fetch(FetchMode.SUBSELECT)
    private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Now, we are going to fetch all `Department` entities that match a given filtering criteria
    and then navigate their `employees` collections.

    </div>
    <div class="paragraph">

    Hibernate is going to avoid the N+1 query issue by generating a single SQL statement to initialize all `employees` collections
    for all `Department` entities that were previously fetched.
    Instead of using passing all entity identifiers, Hibernate simply reruns the previous query that fetched the `Department` entities.

    </div>
    <div id="fetching-strategies-fetch-mode-subselect-example" class="exampleblock">
    <div class="title">Example 295. `FetchMode.SUBSELECT` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Department&gt; departments = entityManager.createQuery(
        "select d " +
        "from Department d " +
        "where d.name like :token", Department.class )
    .setParameter( "token", "Department%" )
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
    where
        d.name like 'Department%'

    -- Fetched 2 Departments

    SELECT
        e.department_id as departme3_1_1_,
        e.id as id1_1_1_,
        e.id as id1_1_0_,
        e.department_id as departme3_1_0_,
        e.username as username2_1_0_
    FROM
        Employee e
    WHERE
        e.department_id in (
            SELECT
                fetchmodes0_.id
            FROM
                Department fetchmodes0_
            WHERE
                d.name like 'Department%'
        )`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">