 ### 11.11. `FetchMode.JOIN`

    <div class="paragraph">

    To demonstrate how `FetchMode.JOIN` works, we are going to modify the [`FetchMode.SELECT` mapping example](#fetching-strategies-fetch-mode-select-mapping-example) to use
    `FetchMode.JOIN` instead:

    </div>
    <div id="fetching-strategies-fetch-mode-join-mapping-example" class="exampleblock">
    <div class="title">Example 296. `FetchMode.JOIN` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "department")
    @Fetch(FetchMode.JOIN)
    private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Now, we are going to fetch one `Department` and navigate its `employees` collections.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The reason why we are not using a JPQL query to fetch multiple `Department` entities is because
    the `FetchMode.JOIN` strategy would be overridden by the query fetching directive.

    </div>
    <div class="paragraph">

    To fetch multiple relationships with a JPQL query, the `JOIN FETCH` directive must be used instead.

    </div>
    <div class="paragraph">

    Therefore, `FetchMode.JOIN` is useful for when entities are fetched directly, via their identifier or natural-id.

    </div>
    <div class="paragraph">

    Also, the `FetchMode.JOIN` acts as a `FetchType.EAGER` strategy.
    Even if we mark the association as `FetchType.LAZY`, the `FetchMode.JOIN` will load the association eagerly.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate is going to avoid the secondary query by issuing an OUTER JOIN for the `employees` collection.

    </div>
    <div id="fetching-strategies-fetch-mode-join-example" class="exampleblock">
    <div class="title">Example 297. `FetchMode.JOIN` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Department department = entityManager.find( Department.class, 1L );

    log.infof( "Fetched department: %s", department.getId());

    assertEquals( 3, department.getEmployees().size() );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        d.id as id1_0_0_,
        e.department_id as departme3_1_1_,
        e.id as id1_1_1_,
        e.id as id1_1_2_,
        e.department_id as departme3_1_2_,
        e.username as username2_1_2_
    FROM
        Department d
    LEFT OUTER JOIN
        Employee e
            on d.id = e.department_id
    WHERE
        d.id = 1

    -- Fetched department: 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This time, there was no secondary query because the child collection was loaded along with the parent entity.

    </div>
    </div>
    </div>
    </div>
    <div class="sect1">