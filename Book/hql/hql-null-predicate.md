### 15.41. Nullness predicate

    <div class="paragraph">

    Check a value for nullness.
    Can be applied to basic attribute references, entity references and parameters.
    HQL additionally allows it to be applied to component/embeddable types.

    </div>
    <div id="hql-null-predicate-example" class="exampleblock">
    <div class="title">Example 397. Nullness checking examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// select all persons with a nickname
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.nickName is not null", Person.class )
    .getResultList();

    // select all persons without a nickname
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.nickName is null", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">