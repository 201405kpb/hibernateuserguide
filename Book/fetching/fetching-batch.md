 ### 11.7. Batch fetching

    <div class="paragraph">

    Hibernate offers the [`@BatchSize`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/BatchSize.html) annotation,
    which can be used when fetching uninitialized entity proxies.

    </div>
    <div class="paragraph">

    Considering the following entity mapping:

    </div>
    <div id="fetching-batch-mapping-example" class="exampleblock">
    <div class="title">Example 290. `@BatchSize` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Department")
    public static class Department {

        @Id
        private Long id;

        @OneToMany(mappedBy = "department")
        //@BatchSize(size = 5)
        private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity

    }

    @Entity(name = "Employee")
    public static class Employee {

        @Id
        private Long id;

        @NaturalId
        private String name;

        @ManyToOne(fetch = FetchType.LAZY)
        private Department department;

        //Getters and setters omitted for brevity
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Considering that we have previously fetched several `Department` entities,
    and now we need to initialize the `employees` entity collection for each particular `Department`,
    the `@BatchSize` annotations allows us to load multiple `Employee` entities in a single database roundtrip.

    </div>
    <div id="fetching-batch-fetching-example" class="exampleblock">
    <div class="title">Example 291. `@BatchSize` fetching example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Department&gt; departments = entityManager.createQuery(
        "select d " +
        "from Department d " +
        "inner join d.employees e " +
        "where e.name like 'John%'", Department.class)
    .getResultList();

    for ( Department department : departments ) {
        log.infof(
            "Department %d has {} employees",
            department.getId(),
            department.getEmployees().size()
        );
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        d.id as id1_0_
    FROM
        Department d
    INNER JOIN
        Employee employees1_
        ON d.id=employees1_.department_id

    SELECT
        e.department_id as departme3_1_1_,
        e.id as id1_1_1_,
        e.id as id1_1_0_,
        e.department_id as departme3_1_0_,
        e.name as name2_1_0_
    FROM
        Employee e
    WHERE
        e.department_id IN (
            0, 2, 3, 4, 5
        )

    SELECT
        e.department_id as departme3_1_1_,
        e.id as id1_1_1_,
        e.id as id1_1_0_,
        e.department_id as departme3_1_0_,
        e.name as name2_1_0_
    FROM
        Employee e
    WHERE
        e.department_id IN (
            6, 7, 8, 9, 1
        )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see in the example above, there are only two SQL statements used to fetch the `Employee` entities associated to multiple `Department` entities.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Without `@BatchSize`, you&#8217;d run into a N+1 query issue, so,
    instead of 2 SQL statements, there would be 10 queries needed for fetching the `Employee` child entities.

    </div>
    <div class="paragraph">

    However, although `@BatchSize` is better than running into an N+1 query issue,
    most of the time, a DTO projection or a `JOIN FETCH` is a much better alternative since
    it allows you to fetch all the required data with a single query.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">