 #### 15.16.2. Using DISTINCT with entity queries

    <div class="paragraph">

    `DISTINCT` can also be used to filter out entity object references when fetching a child association along with the parent entities.

    </div>
    <div id="hql-distinct-entity-query-example" class="exampleblock">
    <div class="title">Example 377. Using DISTINCT with entity queries example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; authors = entityManager.createQuery(
        "select distinct p " +
        "from Person p " +
        "left join fetch p.books", Person.class)
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In this case, `DISTINCT` is used because there can be multiple `Books` entities associated to a given `Person`.
    If in the database there are 3 `Persons` in the database and each person has 2 `Books`, without `DISTINCT` this query will return 6 `Persons` since
    the SQL-level result-set size is given by the number of joined `Book` records.

    </div>
    <div class="paragraph">

    However, the `DISTINCT` keyword is passed to the database as well:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT DISTINCT
        p.id as id1_1_0_,
        b.id as id1_0_1_,
        p.first_name as first_na2_1_0_,
        p.last_name as last_nam3_1_0_,
        b.author_id as author_i3_0_1_,
        b.title as title2_0_1_,
        b.author_id as author_i3_0_0__,
        b.id as id1_0_0__
    FROM person p
    LEFT OUTER JOIN book b ON p.id=b.author_id`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In this case, the `DISTINCT` SQL keyword is undesirable since it does a redundant result set sorting, as explained [in this blog post](http://in.relation.to/2016/08/04/introducing-distinct-pass-through-query-hint/).
    To fix this issue, Hibernate 5.2.2 added support for the `HINT_PASS_DISTINCT_THROUGH` entity query hint:

    </div>
    <div id="hql-distinct-entity-query-hint-example" class="exampleblock">
    <div class="title">Example 378. Using DISTINCT with entity queries example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; authors = entityManager.createQuery(
        "select distinct p " +
        "from Person p " +
        "left join fetch p.books", Person.class)
    .setHint( QueryHints.HINT_PASS_DISTINCT_THROUGH, false )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    With this entity query hint, Hibernate will not pass the `DISTINCT` keyword to the SQL query:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
        p.id as id1_1_0_,
        b.id as id1_0_1_,
        p.first_name as first_na2_1_0_,
        p.last_name as last_nam3_1_0_,
        b.author_id as author_i3_0_1_,
        b.title as title2_0_1_,
        b.author_id as author_i3_0_0__,
        b.id as id1_0_0__
    FROM person p
    LEFT OUTER JOIN book b ON p.id=b.author_id`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using the `HINT_PASS_DISTINCT_THROUGH` entity query hint, Hibernate can still remove the duplicated parent-side entities from the query result.

    </div>
    </div>
    </div>
    <div class="sect2">
