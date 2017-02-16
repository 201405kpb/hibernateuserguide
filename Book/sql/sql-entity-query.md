### 17.3. Entity queries

    <div class="paragraph">

    The above queries were all about returning scalar values, basically returning the _raw_ values from the `ResultSet`.

    </div>
    <div id="sql-jpa-entity-query-example" class="exampleblock">
    <div class="title">Example 428. JPA native query selecting entities</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createNativeQuery(
        "SELECT * FROM person", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-query-example" class="exampleblock">
    <div class="title">Example 429. Hibernate native query selecting entities</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createSQLQuery(
        "SELECT * FROM person" )
    .addEntity( Person.class )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Assuming that `Person` is mapped as a class with the columns `id`, `name`, `nickName`, `address`, `createdOn` and `version`,
    the following query will also return a `List` where each element is a `Person` entity.

    </div>
    <div id="sql-jpa-entity-query-explicit-result-set-example" class="exampleblock">
    <div class="title">Example 430. JPA native query selecting entities with explicit result set</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createNativeQuery(
        "SELECT id, name, nickName, address, createdOn, version " +
        "FROM person", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-query-explicit-result-set-example" class="exampleblock">
    <div class="title">Example 431. Hibernate native query selecting entities with explicit result set</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createSQLQuery(
        "SELECT id, name, nickName, address, createdOn, version " +
        "FROM person" )
    .addEntity( Person.class )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">