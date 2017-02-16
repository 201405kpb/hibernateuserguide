 ### 15.43. Between predicate

    <div class="paragraph">

    Analogous to the SQL `BETWEEN` expression,
    it checks if the value is within boundaries.
    All the operands should have comparable types.

    </div>
    <div id="hql-between-predicate-example" class="exampleblock">
    <div class="title">Example 400. Between predicate examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "join p.phones ph " +
        "where p.id = 1L and index(ph) between 0 and 3", Person.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.createdOn between '1999-01-01' and '2001-01-02'", Person.class )
    .getResultList();

    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration between 5 and 20", Call.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name between 'H' and 'M'", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
