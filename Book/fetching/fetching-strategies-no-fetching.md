 ### 11.3. No fetching

    <div class="paragraph">

    For the first use case, consider the application login process for an `Employee`.
    Let&#8217;s assume that login only requires access to the `Employee` information, not `Project` nor `Department` information.

    </div>
    <div id="fetching-strategies-no-fetching-example" class="exampleblock">
    <div class="title">Example 284. No fetching example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Employee employee = entityManager.createQuery(
        "select e " +
        "from Employee e " +
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
    <div class="paragraph">

    In this example, the application gets the `Employee` data.
    However, because all associations from `Employee `are declared as LAZY (JPA defines the default for collections as LAZY) no other data is fetched.

    </div>
    <div class="paragraph">

    If the login process does not need access to the `Employee` information specifically, another fetching optimization here would be to limit the width of the query results.

    </div>
    <div id="fetching-strategies-no-fetching-scalar-example" class="exampleblock">
    <div class="title">Example 285. No fetching (scalar) example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Integer accessLevel = entityManager.createQuery(
        "select e.accessLevel " +
        "from Employee e " +
        "where " +
        "    e.username = :username and " +
        "    e.password = :password",
        Integer.class)
    .setParameter( "username", username)
    .setParameter( "password", password)
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">