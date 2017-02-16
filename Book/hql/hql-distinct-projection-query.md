 #### 15.16.1. Using DISTINCT with SQL projections

    <div class="paragraph">

    For SQL projections, `DISTINCT` needs to be passed to the database because the duplicated entries need to be filtered out before being returned to the database client.

    </div>
    <div id="hql-distinct-projection-query-example" class="exampleblock">
    <div class="title">Example 376. Using DISTINCT with projection queries example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; lastNames = entityManager.createQuery(
        "select distinct p.lastName " +
        "from Person p", String.class)
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When running the query above, Hibernate generates the following SQL query:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT DISTINCT
        p.last_name as col_0_0_
    FROM person p`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    For this particular use case, passing the `DISTINCT` keyword from JPQL/HQL to the database is the right thing to do.

    </div>
    </div>
    <div class="sect3">
