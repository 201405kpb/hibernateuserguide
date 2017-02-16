### 11.4. Dynamic fetching via queries

    <div class="paragraph">

    For the second use case, consider a screen displaying the `Projects` for an `Employee`.
    Certainly access to the `Employee `is needed, as is the collection of `Projects` for that Employee. Information about `Departments`, other `Employees` or other `Projects` is not needed.

    </div>
    <div id="fetching-strategies-dynamic-fetching-jpql-example" class="exampleblock">
    <div class="title">Example 286. Dynamic JPQL fetching example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Employee employee = entityManager.createQuery(
        "select e " +
        "from Employee e " +
        "left join fetch e.projects " +
        "where " +
        "    e.username = :username and " +
        "    e.password = :password",
        Employee.class)
    .setParameter( "username", username)
    .setParameter( "password", password)
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="fetching-strategies-dynamic-fetching-criteria-example" class="exampleblock">
    <div class="title">Example 287. Dynamic query fetching example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();
    CriteriaQuery&lt;Employee&gt; query = builder.createQuery( Employee.class );
    Root&lt;Employee&gt; root = query.from( Employee.class );
    root.fetch( "projects", JoinType.LEFT);
    query.select(root).where(
        builder.and(
            builder.equal(root.get("username"), username),
            builder.equal(root.get("password"), password)
        )
    );
    Employee employee = entityManager.createQuery( query ).getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In this example we have an `Employee `and their `Projects` loaded in a single query shown both as an HQL query and a JPA Criteria query.
    In both cases, this resolves to exactly one database query to get all that information.

    </div>
    </div>
    <div class="sect2">