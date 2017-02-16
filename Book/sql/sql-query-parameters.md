 ### 17.9. Parameters

    <div class="paragraph">

    Native SQL queries support positional as well as named parameters:

    </div>
    <div id="sql-jpa-query-parameters-example" class="exampleblock">
    <div class="title">Example 444. JPA native query with parameters</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createNativeQuery(
        "SELECT * " +
        "FROM person " +
        "WHERE name like :name", Person.class )
    .setParameter("name", "J%")
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-query-parameters-example" class="exampleblock">
    <div class="title">Example 445. Hibernate native query with parameters</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createSQLQuery(
        "SELECT * " +
        "FROM person " +
        "WHERE name like :name" )
    .addEntity( Person.class )
    .setParameter("name", "J%")
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">