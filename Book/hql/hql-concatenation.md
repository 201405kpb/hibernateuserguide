### 15.25. Concatenation (operation)

    <div class="paragraph">

    HQL defines a concatenation operator in addition to supporting the concatenation (`CONCAT`) function.
    This is not defined by JPQL, so portable applications should avoid it use.
    The concatenation operator is taken from the SQL concatenation operator (e.g `||`).

    </div>
    <div id="hql-concatenation-example" class="exampleblock">
    <div class="title">Example 384. Concatenation operation example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`String name = entityManager.createQuery(
        "select 'Customer ' || p.name " +
        "from Person p " +
        "where p.id = 1", String.class )
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    See [Scalar functions](#hql-exp-functions) for details on the `concat()` function

    </div>
    </div>
    <div class="sect2">
